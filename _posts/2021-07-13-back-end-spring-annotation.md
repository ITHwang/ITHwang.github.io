---
layout: single
title: "스프링 애너테이션이 편한 이유"
toc: true
toc_sticky: true
tags:
  - spring
  - java
categories:
  - Back-end
---

스프링 2.5까지는 xml 파일로 다소 복잡하게 DI나 AOP와 같은 기능을 설정해야 했으나, 3.0부터는 애너테이션(Annotation)으로 편하게 설정할 수 있도록 지원하고 있습니다.

따라서 구체적으로 어떤 부분이 편해졌는지 직접 정리해보려합니다.

## 스프링 애너테이션 제공 클래스

---

먼저 스프링 애너테이션을 쓰기 전의 xml 파일을 보겠습니다.

`action-servlet.xml`은 `web.xml`에서 **action**으로 매핑된 `DispatcherServlet`의 설정 파일입니다.

- action-servlet.xml

  ```xml
  <bean id="memberController" class="com.spring.member.controller.MemberControllerImpl">
  <property name="methodNameResolver">
  	<ref local="memberMethodResolver"/>
  </property>
  <property name="memberService" ref="memberService"/>
  </bean>

  <bean id="memberMethodResolver" class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver" >
  	<property name="mappings">
  		<props>
  			<prop key="/member/listMembers.do">listMembers</prop>
  			<prop key="/member/addMember.do">addMember</prop>
  			<prop key="/member/removeMember.do">removeMember</prop>
  			<prop key="/member/memberForm.do">form</prop>
  			<prop key="/member/modMemberForm.do">modMemberForm</prop>
  			<prop key="/member/updateMember.do">updateMember</prop>
  		</props>
  	</property>
  </bean>

  <bean id="memberUrlMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
  	<property name="mappings">
  		<props>
  			<prop key="/member/*.do">memberController</prop>
  		</props>
  	</property>
  </bean>
  ```

**memberUrlMapping**에 모든 요청(\*.do)에 대해 **memberController**가 받도록 매핑되어 있으며, **memberController** 내 **memberMethodResolver**가 각 세부 요청을 메서드와 매핑하고 있습니다.

즉 xml 파일을 통해 controller 구현 복잡도를 줄였지만, 기능이 많이 늘어난다면 xml 파일 설정도 꽤 복잡해질 것입니다.

그러나 스프링 애너테이션이 제공하는 관련 클래스들을 활용하여 다음과 같이 코드를 줄일 수 있게 되었습니다.

- action-servlet.xml

  ```xml
  <bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>
  <bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>
  <context:component-scan base-package="com.spring"/>
  ```

`DefaultAnnotationHandlerMapping`은 클래스 레벨에서 **@RequestMapping**을 처리하고, `AnnotationMethodHandlerAdapter`는 메서드 레벨에서 **@RequestMapping**을 처리합니다.

두 클래스를 활용하여 다음과 같이 Controller 클래스를 작성할 수 있습니다.

- MemberControllerImpl.java

  ```java
  	...

  @Controller("memberController")
  public class MemberControllerImpl extends MultiActionController implements MemberController {

  	...

  	@Override
  	@RequestMapping(value = "/member/listMembers.do", method = RequestMethod.GET)
  	public ModelAndView listMembers(HttpServletRequest request, HttpServletResponse response) throws Exception {
  			...
  	}

  	@Override
  	@RequestMapping(value = "/member/addMember.do", method = { RequestMethod.GET, RequestMethod.POST })
  	public ModelAndView addMember(@ModelAttribute("member") MemberVO member, HttpServletRequest request,
  			HttpServletResponse response) throws Exception {
  		request.setCharacterEncoding("utf-8");
  			...
  	}

  	@Override
  	@RequestMapping(value = "/member/removeMember.do", method = RequestMethod.GET)
  	public ModelAndView removeMember(@RequestParam("id") String id, HttpServletRequest request,
  			HttpServletResponse response) throws Exception {
  			...
  	}
  }
  ```

즉, 이전에 **memberController**과 그 안에 있는 메서드들을 xml 파일로 매핑했다면, 애너테이션을 사용할 때는 클래스나 메서드 윗 줄에 **@RequestMapping**을 붙임으로써 간편하게 설정할 수 있게 되었습니다.

또한 클래스 위에 `@Controller("memberController")`를 붙인 것처럼, **context:component-scan** 태그로 패키지를 지정하고 그 패키지 안에 있는 클래스에 애너테이션을 붙인다면 자동으로 빈을 생성해줍니다.

**context:component-scan** 태그가 지원하는 애너테이션은 <u>@Controller, @Service, @Repository, @Component</u>가 있습니다.

## @Autowired

---

한 클래스 내에 다른 클래스를 직접 생성한다면 코드 관리가 힘들어지므로, xml 파일을 이용해 스프링 DI 기능을 이용해왔습니다.

그러나 이 또한 코드가 계속 늘어난다면 xml 파일이 복잡해지면서 유지보수가 힘들어지는 것은 마찬가지일 것입니다.

이에 스프링에서는 **@Autowired**를 이용하여 코드 내 애너테이션만으로 DI가 가능하도록 지원하고 있습니다.

Controller, Service, DAO에 대하여 원래 다음과 같이 xml 파일로 의존성 주입을 설정했었습니다.

- action-mybatis.xml

  ```xml
  <bean id="memberDAO"
  	class="com.spring.member.dao.MemberDAOImpl">
  	<property name="sqlSession" ref="sqlSession"></property>
  </bean>
  ```

- action-service.xml

  ```xml
  <bean id="memberService" class="com.spring.member.service.MemberServiceImpl">
      <property name="memberDAO" ref="memberDAO"/>
    </bean>
  ```

- action-sevlet.xml

  ```xml
  <bean id="memberController" class="com.spring.member.controller.MemberControllerImpl">
       	<property name="methodNameResolver">
       		<ref local="memberMethodResolver"/>
       	</property>
       	<property name="memberService" ref="memberService"/>
    </bean>
  ```

이는 xml 파일이 복잡해질뿐더러 별도의 setter나 생성자도 필요로 합니다.
그러나 **@Autowired**를 이용한다면 xml 파일, setter, 생성자 없이 코드에서 편하게 설정할 수 있습니다.

- MemberDAOImpl.java

  ```java
  @Repository("memberDAO")
  public class MemberDAOImpl implements MemberDAO {
  @Autowired
  private SqlSession sqlSession;

  ...

  }
  ```

- MemberServiceImpl.java

  ```java
  @Service("memberService")
  @Transactional(propagation = Propagation.REQUIRED)
  public class MemberServiceImpl implements MemberService {
  @Autowired
  private MemberDAO memberDAO;

  ...

  }
  ```

- MemberControllerImpl.java

  ```java
  @Controller("memberController")
  public class MemberControllerImpl extends MultiActionController implements MemberController {
  @Autowired
  private MemberService memberService;
  @Autowired
  private MemberVO memberVO;

  ...

  }
  ```

참고로 `@Repository("memberDAO")`, `@Service("memberService")`, `@Controller("memberController")`는 위에서 말씀드린 **context:component-scan**에 해당합니다.

다만 스프링에서 제공하는 클래스를 사용하려면 여전히 xml로 설정해야 하며, 개발자가 만든 클래스만 자바 코드에서 애너테이션으로 설정할 수 있습니다.

## Conclusion

---

스프링 애너테이션을 이용하면 구체적으로 무엇이 편해지는지에 대해 코드로 직접 정리해보았습니다.
감사합니다!!

## Reference

---

- 이병승, 『자바 웹을 다루는 기술』, 길벗, 2019, 26장

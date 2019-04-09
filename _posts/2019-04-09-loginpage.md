---
layout: post
title: login page만들기
date: 2019-04-09
categories: [MeetingApp]
---

## login 페이지 만들기
로그인 화면은 form 태그를 사용해서 구성한다. django에서 form태그를 쉽게 구성할 수 있도록 제공하는 FormView를 이용한다.
{% highlight vim %}
#loginform.py

from django import forms

class LoginForm(forms.Form):
  Id = forms.CharField(label='ID', max_length=20)
  password = forms.CharField(label='Password', max_length=30)
  
#views.py
from app.loginform import LoginForm
from django.views.generic.edit import FormView

class LoginFormView(FormView):
  form_class = LoginForm
  template_name = 'app/login.html'
  success_url = '.'       #redirection 시킬 url
  
  def form_valid(self, form):
    return super(LoginFormView, self).form_valid(form) #유효한 폼 데이터로 처리하는 로직.(success_url로 리다이렉션됨)
{% endhighlight %}
이제 실제 화면을 출력해주는 login.html을 작성한다. {{ form.as_p }}는 위의 LoginForm을 렌더링한 부분이다. as_p는 렌더링 결과를 <p>태그로 감싼다.
```html
<form action="/app/login" method="post">
   {% csrf_token %}
   {{ form.as_p }}
    <input type="submit" value="로그인"/>
</form>
```
[참고] 만약 TemplateDoesNotExist at 에러 발생하면 templates 폴더이름도 확인해볼것(template이라고 해서 에러났었음..)


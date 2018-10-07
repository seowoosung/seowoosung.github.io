---
layout: post
title: Character set과 Character encoding 정리
---

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

![_config.yml]({{ site.baseurl }}/images/unicode.png)


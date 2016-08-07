---
layout: post
title:  "Some details about Qt properties system"
date:   2016-08-07 21:18:00 +0300
categories: researches
---

As I promised, in this post I shall tell you about some details of Qt properties system. Most probably, you'll find it useful if you are only familiar but haven't used Qt properties much in your code yet. Using them is a good way to start integrating your application to QtQuick, where, as far as I know, Qt properties are mostly used. Correct me, if I'm not accurate. I shan't explain you what Qt properties are, so if you don't know about them, you may want to read the following article: <a href="http://doc.qt.io/qt-5/properties.html" target="_blank">http://doc.qt.io/qt-5/properties.html</a>. This post actually will be quite short.

When reading the documentation, I've found out for myself that getters and setters are not properly explained. My main question was:<BR>
"Will be my custom READ and WRITE functions called if I use `obj->property(...)` or `obj->setProperty(..., ...)` in the code?"<BR>
Spoiler: the short answer is yes.

In my project I use the following code:

{% highlight cpp %}
    Q_PROPERTY(QString fullPath MEMBER m_fullPath WRITE setFullPath)
...
public:
    void setFullPath(const QString &fullPath);
...
private:
    QString m_fullPath;
...
}
...
void Archive::Entry::setFullPath(const QString &fullPath)
{
    m_fullPath = fullPath;
    const QStringList pieces = m_fullPath.split(QLatin1Char( '/' ), QString::SkipEmptyParts);
    m_name = pieces.isEmpty() ? QString() : pieces.last();
}
{% endhighlight %}

So calling both
{% highlight bash %}
entry->setProperty("fullPath", "some/path");
{% endhighlight %}
and
{% highlight bash %}
entry->setFullPath("some/path");
{% endhighlight %}
will bring desirable side effects. The same applies to READ functions.

That's all. Thanks for reading!

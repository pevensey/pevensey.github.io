---
layout: post
title: Syntax Highlighting Post
date: 2013-08-16
excerpt: "Demo post displaying the various ways of highlighting code in Markdown."
tags: [sample post, code, highlighting]
comments: true
---

Syntax highlighting is a feature that displays source code, in different colors and fonts according to the category of terms. This feature facilitates writing in a structured language such as a programming language or a markup language as both structures and syntax errors are visually distinct. Highlighting does not affect the meaning of the text itself; it is intended only for human readers.[^1]

[^1]: <http://en.wikipedia.org/wiki/Syntax_highlighting>

### Highlighted Code Blocks

To modify styling and highlight colors edit `/assets/css/syntax.css`.

{% highlight css %}
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
{% endhighlight %}

{% highlight python %}

import math
import numpy as np

print("_______________________________________")
print("Algoritma RSA - Enkripsi dan Dekripsi")

# Input Prime Numbers
print("_______________________________________")
print("Input bilangan prima p dan q")
p = int(input("Input bilangan prima p : "))
q = int(input("Input bilangan prima q : "))
print("_______________________________________")


# Fungsi untuk mengecek apakah bilangan p dan q yang di inputkan benar bilangan prima
def prime_check(a):
    if (a == 2):
        return True
    elif ((a < 2) or ((a % 2) == 0)):
        return False
    elif (a > 2):
        for i in range(2, a):
            if not (a % i):
                return False
    return True


check_p = prime_check(p)
check_q = prime_check(q)
while (((check_p == False) or (check_q == False))):
    p = int(input("Input bilangan prima p : "))
    q = int(input("Input bilangan prima q : "))
    check_p = prime_check(p)
    check_q = prime_check(q)

# Modulus dari n
n = p * q
print("_______________________________________")
print("RSA Modulus(n)    :", n)

# Fungsi Eulers Toitent
'''Menghitung fungsi eluers toitent dari 'r'.'''
r = (p - 1) * (q - 1)

print("Eulers Toitent(r) :", r)
print("_______________________________________")

# Fungsi untuk menghitung FPB (e, r) = 1

def egcd(e, r):
    while (r != 0):
        e, r = r, e % r
    return e


# Algoritma Euclidean
def eugcd(e, r):
    for i in range(1, r):
        while (e != 0):
            a, b = r // e, r % e
            if (b != 0):
                print("%d = %d*(%d) + %d" % (r, a, e, b))
            r = e
            e = b


# Extended Algoritma Euclidean
def eea(a, b):
    if (a % b == 0):
        return (b, 0, 1)
    else:
        gcd, s, t = eea(b, a % b)
        s = s - ((a // b) * t)
        print("%d = %d*(%d) + (%d)*(%d)" % (gcd, a, t, s, b))
        return (gcd, t, s)


# Multiplicative Inverse
def mult_inv(e, r):
    gcd, s, _ = eea(e, r)
    if (gcd != 1):
        return None
    else:
        if (s < 0):
            print("s=%d. Since %d is less than 0, s = s(modr), i.e., s=%d." % (s, s, s % r))
        elif (s > 0):
            print("s=%d." % (s))
        return s % r


# e Value Calculation
# Mencari nilai coprime atau nilai e yang FPB(e,r) = 1
for i in range(1, 1000):
    if (egcd(i, r) == 1):
        e = i
print("Nilai e adalah :", e)
print("_______________________________________")

# d, Private dan Public Key
# menghitung nilai dari d yang digunakan untuk membangkitkan Private dan Public Key
print("Hasil dari Euclidean Algoritma untuk prima e dan r yang relatif 1 :")
eugcd(e, r)
print("End Euclidean Algoritma : ")
print("_______________________________________")

print("Algoritma perpanjangan Euclidean")
d = mult_inv(e, r)

print("Langkah terakhir untuk mendapatkan nilai d")
print("Nilai D adalah :", d)
print("_______________________________________")

public = (e, n)
private = (d, n)

np.savetxt("public_key", np.array([public], ), fmt="%s")
np.savetxt("private_key", np.array([private], ), fmt="%s")

print("Private Key adalah :", private)
print("Public Key adalah  :", public)
print("_______________________________________")

# Fungsi Enkripsi
def encrypt(pub_key, n_text):
    e, n = pub_key
    x = []
    m = 0
    for i in n_text:
        if (i.isupper()):
            m = ord(i) - 65
            c = (m ** e) % n
            x.append(c)
        elif (i.islower()):
            m = ord(i) - 97
            c = (m ** e) % n
            x.append(c)
        elif (i.isspace()):
            spc = 400
            x.append(400)
    return x


# Fungsi Dekripsi
def decrypt(priv_key, c_text):
    d, n = priv_key
    txt = c_text.split(' ')
    x = ''
    m = 0
    for i in txt:
        if i == '400':
            x += ' '
        else:
            m = (int(i) ** d) % n
            m += 97
            c = chr(m)
            x += c
        # elif(i.islower() == True):
        #     m= (int(i) ** d) % n
        #     m+= 97
        #     c= chr(m)
        #     x+= c
    return x

while True:
    # Choose Encrypt or Decrypt and Print
    
    print(" Pilih apakah ingin melakukan enkripsi atau dekripsi pesan")
    print(" 1. Enkripsi")
    print(" 2. Dekripsi")
    print(" 3. Exit")
    choose = input("Masukan pilihan : ")


    if choose == '1':

        # Pesan yang di enkripsi dari file txt
        f = open('pesan.txt', 'r')
        content = f.read()
        # print(content)
        message = content
        print("Pesan yang di enkripsi adalah :", message)
        f.close()

        enc_msg = encrypt(public, message)
        print("Pesan yang telah di enkripsi adalah :", enc_msg)

        np.savetxt("hasil_enkripsi.txt", np.array([enc_msg], ), fmt="%s")
        # g = open('hasil_enkripsi.txt', 'w')
        # g.write(enc_msg)
        # g.close()

        print("Enkripsi file menggunakan RSA berhasil, silahkan cek pada file hasil_enkripsi.txt")
        print("_________________________________________________________________________________")

    elif choose == '2':

        # Message
        f = open('hasil_enkripsi.txt', 'r')
        content = f.read()
        # print(content)
        message = content
        print("Pesan yang telah di enkripsi :", message)
        f.close()

        dekripsi = decrypt(private, message)
        print("Pesan yang telah di dekripsi :", dekripsi)

        g = open('hasil_dekripsi.txt', 'w')
        g.write(dekripsi)
        g.close()

        # np.savetxt("hasil_dekripsi.txt", np.array([dekripsi], ), fmt="%s")

        print("Dekripsi menggunakan RSA berhasil, silahkan cek pada hasil_dekripsi.txt")
        print("_________________________________________________________________________________")

        
    elif choose == '3':
        exit()
    else:
        print(" ")
        exit()

{% endhighlight %}

{% highlight ruby %}
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
{% endhighlight %}


### Standard Code Block

    {% raw %}
    <nav class="pagination" role="navigation">
        {% if page.previous %}
            <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
        {% endif %}
        {% if page.next %}
            <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
        {% endif %}
    </nav><!-- /.pagination -->
    {% endraw %}


### Fenced Code Blocks

To modify styling and highlight colors edit `/assets/css/syntax.css`. Line numbers and a few other things can be modified in `_config.yml`. Consult [Jekyll's documentation](http://jekyllrb.com/docs/configuration/) for more information.

~~~ css
#container {
    float: left;
    margin: 0 -240px 0 0;
    width: 100%;
}
~~~

~~~ html
{% raw %}<nav class="pagination" role="navigation">
    {% if page.previous %}
        <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
    {% endif %}
    {% if page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
    {% endif %}
</nav><!-- /.pagination -->{% endraw %}
~~~

~~~ ruby
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
~~~

### GitHub Gist Embed

An example of a Gist embed below.

{% gist mmistakes/6589546 %}

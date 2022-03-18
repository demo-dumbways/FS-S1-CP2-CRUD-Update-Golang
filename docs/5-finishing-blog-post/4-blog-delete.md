---
sidebar_position: 4
---

# 4. Blog Delete using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Hal pertama yang kita lakukan adalah menambahkan id blog yang akan dihapus pada file `blog.hbs` bagian `tag anchor` button delete. Hal ini bertujuan untuk nantinya pada route delete dapat menerima id blog mana yang akan hapus dari database.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-3.delete-blog-sql/views/blog.hbs">
Contoh code
</a>

<br />
<br />

```html title=blog.hbs {9}
{{#each blogs}}
    <div class="blog-list-item">
      <div class="blog-image">
        <img src="/public/assets/blog-img.png" alt="Pasar Coding di Indonesia Dinilai Masih Menjanjikan" />
      </div>
      <div class="blog-content">
        <div class="button-group">
          <a class="btn-edit">Edit Post</a>
          <a href="/delete-blog/{{this.id}}" class="btn-post">Delete Blog</a>
        </div>
        <h1>
          <a href="/blog/{{this.id}}" target="_blank">{{this.title}}</a>
        </h1>
        <div class="detail-blog-content">
          {{this.post_date}} | {{this.author}}
        </div>
        <p>{{this.content}}</p>
      </div>
    </div>
{{/each}}
```
Kita akan menambahkan sebuah route baru pada file `index.js`. Route kita berikan endpoint `delete-blog/:id`, sesuai dengan tujuannya yakni menghapus data blog dari database. Query yang akan digunakan untuk menghapus data yakni `DELETE`.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-3.delete-blog-sql/api/index.js">
Contoh code
</a>

<br />
<br />

```js title=index.js {3-16}
// this code below endpoint app.post('/blog', .....

app.get('/delete-blog/:id', function (req, res) {
    let id = req.params.id
    let query = `DELETE FROM blog WHERE id = ${id}`

    setHeader(res)
    db.connect(function (err, client, done) {
        done()
        if (err) throw err
        client.query(query, function (err, result) {
            if (err) throw err
            res.redirect('/blog')
        })
    })
})

app.get('/contact-me', function (req, res) {
    setHeader(res)
    res.render('contact')
})
```

Mengambil index yang dikirim, kita gunakan `req.params.index`, jadi index tersebut tersimpan didalam `req.params`. Selanjutnya id yang kita dapatkan akan menjadi kondisi pada query yang dijalankan. Kondisi yang dimaksud yakni hanya menghapus data blog yang id nya sama dengan id yang dikirimkan.

Sebenarnya penulisan query yang digunakan bisa kita `simpan kedalam sebuah variabel` terlebih dahulu seperti endpoint delete ini, ataupun `langsung dituliskan` seperti yang dituliskan pada endpoint detail blog dan blog post.

<img alt="image1" src={useBaseUrl('img/docs/image-5-5.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="https://personal-web-chapter-2.herokuapp.com/blog">
Demo
</a>
</div>
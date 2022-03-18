---
sidebar_position: 3
---

# 3. Blog Post using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Pada pertemuan kedua kita telah membuat sebuah route menggunakan metode `POST` dengan nama endpoint `blog` untuk menyimpan data inputan dari formulir blog. Hanya saja pada pertemuan tersebut kita masih sebatas menyimpan data pada variabel. Kali ini kita akan menyimpan data kedalam database menggunakan `query INSERT`.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-2.blog-post-sql/api/index.js">
Contoh code
</a>

<br />
<br />

```js title=index.js {6-19}
app.get('/add-blog', function (req, res) {
    setHeader(res)
    res.render("form-blog")
})

app.post('/blog', function (req, res) {
    let data = req.body

    let query = `INSERT INTO blog(title, content, image) VALUES ('${data.title}', '${data.content}', 'image.png')`

    db.connect(function (err, client, done) {
        if (err) throw err

        client.query(query, function (err, result) {
            if (err) throw err
            res.redirect('/blog')
        })
    })
})

app.get('/delete-blog/:index', (req, res) => {
    const index = req.params.index;

    blogs.splice(index, 1);

    setHeader(res)
    res.redirect('/blog');
});

```

Pada saat memasukkan data inputan yang kita terima kedalam database, maka kita perlu mespesifikkan column mana saja yang akan kita inputkan. Pada study case kali ini, kita belum menangani terkait `file upload`. Hal tersebut belum dilakukan karena membutuhkan package tambahan dengan nama `Multer` yang nantinya akan kita bahas pada pertemuan Day 7 terkait `PostgreSQL Relation and File Upload in Node JS`. Sehingga pada case kali ini kita masih menggunakan data statis untuk mengisi data pada colum `image`.

<img alt="image1" src={useBaseUrl('img/docs/image-5-3.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="https://personal-web-chapter-2.herokuapp.com/add-blog">
Demo
</a>
</div>

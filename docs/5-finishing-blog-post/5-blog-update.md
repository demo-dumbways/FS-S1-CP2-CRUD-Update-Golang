---
sidebar_position: 5
---

# 5. Blog Update using PostgreSQL Query (Challange)

Hal pertama yang kita lakukan adalah menambahkan id blog yang akan diupdate pada file `blog.hbs` bagian `tag anchor` button edit. Hal ini bertujuan untuk nantinya pada route delete dapat menerima id blog mana yang akan diupdate dari database.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-4.update-blog-sql/views/blog.hbs">
Contoh code
</a>

<br />
<br />

```html title=blog.hbs {8}
{{#each blogs}}
    <div class="blog-list-item">
      <div class="blog-image">
        <img src="/public/assets/blog-img.png" alt="Pasar Coding di Indonesia Dinilai Masih Menjanjikan" />
      </div>
      <div class="blog-content">
        <div class="button-group">
          <a href="/update-blog/{{this.id}}" class="btn-edit">Edit Post</a>
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

Ketika kita ingin mengupdate data dari database, sebaiknya terdapat 2 langkah yang harus dilakukan. 
1.  Get Data Blog 

    Langkah pertama yakni mendapatkan data yang akan diupdate, agar nantinya pengguna tidak perlu menginputkan ulang untuk data data yang tidak ingin diubah. 

    Oleh karena itu kita akan menambahkan sebuah route baru pada file `index.js`. Route kita berikan endpoint `update-blog/:id` dengan metode `GET`. Pada route ini nantinya kita akan menjalankan query `SELECT` yang akan merender tampilan form update blog beserta dengan `value` inputan yang telah terisi

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-4.update-blog-sql/api/index.js">
    Contoh code
    </a>

    <br />
    <br />

    ```js title=index.js
    // this code below endpoint app.get('/delete-blog/:id', .....
    app.get('/update-blog/:id', function (req, res) {
        let id = req.params.id
        let query = `UPDATE FROM blog WHERE id = ${id}`

        setHeader(res)
        db.connect(function (err, client, done) {
            done()
            if (err) throw err
            client.query(query, function (err, result) {
                if (err) throw err
                res.render('update-blog', { isLogin: isLogin, blog: result.rows[0] })
            })
        })
    })
    ```

    selanjutnya adalah membuat sebuah view yang akan dirender untuk `update blog`. View ini berisikan form yang serupa dengan form add blog post, hanya saja sudah kita berikan `default value` yang berasal dari database

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-4.update-blog-sql/views/update-blog.hbs">
    Contoh code
    </a>

    <br />
    <br />

    ```html title=update-blog.hbs
    <!DOCTYPE html>
    <html>
    <head>
        <title>Blog Form With Bootstrap</title>
        <link rel="stylesheet" href="/public/style.css">
        <!-- linking boostrap css cdn  -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    </head>
    <body>
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-lg">
            <a class="navbar-brand me-5" href="/home">
            <img src="/public/assets/logo.png" alt="logo" />
            </a>
            <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
                <li class="nav-item">
                <a class="nav-link" href="index.html">Home</a>
                </li>
                <li class="nav-item">
                <a href="blog.html" class="nav-link list-active">Blog</a>
                </li>
            </ul>
            </div>
            <div class="d-flex contact-me">
            <a href="contact.html">Contact Me</a>
            </div>
        </div>
        </nav>
        <form class="form-container" action="/update-blog/{{blog.id}}" method="POST">
        <h1>Create Post Blog</h1>
        <div>
            <label for="input-title" class="form-label">Title</label>
            <input id="input-title" name="title" class="form-control" value="{{blog.title}}" />
        </div>
        <div>
            <label for="input-content" class="form-label">Content</label>
            <textarea id="input-content" name="content" class="form-control">{{blog.content}}</textarea>
        </div>
        <div>
            <label for="input-image" class="form-label">Upload Image</label>
            <input class="form-control" type="file" id="input-image">
        </div>
        <div class="d-flex justify-content-end">
            <button type="submit" class="btn">Update Blog</button>
        </div>
        </form>
    </body>
    </html>
    ```

2.  Post Data Blog Update

    Langkah kedua yang dilakukan adalah mengirimkan data blog update ke database menggunakan query `UPDATE` berdasarkan id yang dikirimkan melalui form update. Kita akan membuat sebuah route baru dengan nama `update-blog/:id` menggunakan metode `POST`. 

    <a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-4.update-blog-sql/api/index.js">
    Contoh code
    </a>

    <br />
    <br />

    ```js title=index.js {2-17}
    // this code below endpoint app.get('/update-blog/:id', .....
    app.post('/update-blog/:id', function (req, res) {
        let id = req.params.id
        let data = req.body

        let query = `UPDATE blog SET title='${data.title}', content='${data.content}' WHERE id='${id}'`

        db.connect(function (err, client, done) {
            if (err) throw err

            client.query(query, function (err, result) {
                done()
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
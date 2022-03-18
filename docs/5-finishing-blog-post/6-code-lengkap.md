---
sidebar_position: 6
---

# 6. Full Code Index.JS
Berikut adalah Full Code pada file `index.js` :

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-5.full-code/api/index.js">
Contoh code
</a>

<br />
<br />

```js title="index.js"
const express = require('express')
const path = require("path");

const db = require(path.join(__dirname, '../connection/db'));

const app = express()

app.set('view engine', 'hbs');
app.set("views", path.join(__dirname, "../views"));

app.use("/public", express.static(path.join(__dirname, "../public")));

app.use(express.urlencoded({ extended: false }))

const blogs = [
    {
        id: 1,
        title: 'Pasar Coding di Indonesia Dinilai Masih Menjanjikan',
        post_date: '12 Jul 2021 22:30 WIB',
        author: 'Ichsan Emrald Alamsyah',
        content: `Ketimpangan sumber daya manusia (SDM) di sektor digital masih
                    menjadi isu yang belum terpecahkan. Berdasarkan penelitian
                    ManpowerGroup, ketimpangan SDM global, termasuk Indonesia,
                    meningkat dua kali lipat dalam satu dekade terakhir. Lorem ipsum,
                    dolor sit amet consectetur adipisicing elit. Quam, molestiae
                    numquam! Deleniti maiores expedita eaque deserunt quaerat! Dicta,
                    eligendi debitis?`,
    },
];

app.get('/', function (req, res) {
    res.send("Hello World")
})

app.get('/home', function (req, res) {
    setHeader(res)
    res.render('index')
})

const isLogin = true

app.get('/blog', function (req, res) {
    setHeader(res)

    db.connect((err, client, done) => {
        if (err) throw err

        client.query('SELECT * FROM tb_blog', (err, result) => {
            done()
            if (err) throw

            let data = result.rows

            data = data.map((blog) => {
                return {
                    ...blog,
                    post_at: getFullTime(blog.post_at),
                    post_age: getDistanceTime(blog.post_at),
                    isLogin: isLogin
                }
            })

            res.render(
                'blog',
                {
                    isLogin: isLogin,
                    blogs: data
                })
        })
    })
})

app.get('/blog/:id', function (req, res) {
    const blogId = req.params.id

    setHeader(res)
    db.connect((err, client, done) => {
        if (err) throw err

        client.query(`SELECT * FROM blog WHERE id = ${id}`, function (err, result) {
            done()
            if (err) throw err

            res.render('blog-detail', { isLogin: isLogin, blog: result.rows[0] })
        })
    })
})

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

const port = 5000
app.listen(port, function () {
    console.debug(`Server running on port ${port}`)
})

function setHeader(res) {
    res.setHeader("Content-Type", "text/html");
    res.setHeader("Cache-Control", "s-max-age=1, stale-while-revalidate");
}
```

# **Theater CMS Live Project**

## _Introduction_

I participated in a Live Project with a team of peers for the final two weeks of my C# and .NET course at The Tech Academy.
The project was for a company in Portland called Theatre Vertigo. It involved creating and building an interactive website for managing and productions using a Code-first approach with Entity Framework and .NET MVC.

This team project was organized into a two-week sprint and was managed with Agile and Scrum methodologies. Organization included an inception planning meeting, daily stand-ups, weekly sprint reviews, and finished with a code retrospective. We used Azure DevOps, and employed boards and user stories to achieve a well-ordered level of Project Management.

The majority of my work for this project revolved around creating a Blog area for the website, with user and admin capabilities. I was assigned some of the harder stories for this project and am very proud of what I was able to learn and accomplish.

<br />
<hr>

## _My Assignments_

Front End Stories
-----------------

* [Dynamically Display Developer Names](#1-dynamically-display-developer-names)
* [Style the Comment Section](#4-style-the-comment-section)

Back End Stories
----------------
* [Create Comment Model and CRUD Pages](#2-create-comment-model-and-crud-pages)

Full Stack Stories
------------------
* [Create and Implement Partial View for Blog Posts](#3-create-and-implement-partial-view-for-blog-posts)
* [Implementing Comment Feature Functionality](#5-implementing-comment-feature-functionality)
* [Create A Like Ratio Progress Bar](#6-create-a-like-ratio-progress-bar)


<hr><hr>

### 1. Dynamically Display Developer Names
My first assignment was to use JavaScript/jQuery and bootstrap to count the number developers by name that have worked on this project and display that number next to the title heading.

![After](/images/Story1_counter.png)

```c#
/* SIGN-IN PAGE */

<div class="py-5 text-center">
    @*Centered text and padded bottom*@
    <h1>Developers Of TheatreCMS&nbsp;<span id="NumPersons" class="badge badge-secondary">Total</span></h1>
    <h2>SignIn</h2>
</div>
```
```javascript
/* JS FOR SIGN-IN PAGE */
$(document).ready(function () {
    var numberOfDevelopers = $('#PersonList p').length;
    $('#NumPersons').text(numberOfDevelopers);
});
```
Back to: [Assignments](#my-assignments)
<br />

### 2. Create Comment Model and CRUD Pages
Next, I was tasked with creating a model for the Blog and creating the database table for it through Entity Framework. This model represented a user's comment on a blog post. It involved creating a class and associated properties to match the dB schema, context file implementation for table creation, and creating a new controller with scaffolding for CRUD pages for table management. By referencing a UML class diagram, I was able to create a Comment constructor with the current date/time, the comment author, their message, and likes and dislikes for the comment. After I created the model I scaffolded the CRUD pages using Visual Studio and EntityFramework to create the Index, Edit, Create, Details, and Delete pages for the admins and users.
<br />

UML Class Diagram: <br />
![UML Diagram](/images/Story2_UML-class-diagram.png)

Comment Model:
```c#
//Comment.cs            
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using TheatreCMS3.Models;

//Creates a model in the Blog area called Comment | model will represent a user's comment on a blog post
namespace TheatreCMS3.Areas.Blog
{
    public class Comment
    {
        //defining the Comment class with appropriate properties and methods
        public int CommentId { get; set; }
        public ApplicationUser Author { get; set; }
        public string Message { get; set; }
        public DateTime CommentDate { get; set; }
        public int Likes { get; set; }
        public int Dislikes { get; set; }

        public Comment()
        {
            CommentDate = DateTime.Now;
        }
        public double LikeRatio()
        {
            if (Likes + Dislikes == 0)
            {
                return 0;
            }
            return (double)Likes / (Likes + Dislikes);
        }
    }
}
```

Comments Controller:
```c#
namespace TheatreCMS3.Areas.Blog.Controllers
{
    public class CommentsController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // GET: Blog/Comments
        public ActionResult Index()
        {
            return View(db.Comments.ToList());
        }
        // GET: Blog/Comments/Details/5
        public ActionResult Details(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Comment comment = db.Comments.Find(id);
            if (comment == null)
            {
                return HttpNotFound();
            }
            return View(comment);
        }
        // GET: Blog/Comments/Create
        public ActionResult Create()
        {
            return View();
        }
        // POST: Blog/Comments/Create
        // To protect from overposting attacks, enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "CommentId,Message,CommentDate,Likes,Dislikes")] Comment comment)
        {
            if (ModelState.IsValid)
            {
                db.Comments.Add(comment);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(comment);
        }
        // GET: Blog/Comments/Edit/5
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Comment comment = db.Comments.Find(id);
            if (comment == null)
            {
                return HttpNotFound();
            }
            return View(comment);
        }
        // POST: Blog/Comments/Edit/5
        // To protect from overposting attacks, enable the specific properties you want to bind to, for 
        // more details see https://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "CommentId,Message,CommentDate,Likes,Dislikes")] Comment comment)
        {
            if (ModelState.IsValid)
            {
                db.Entry(comment).State = EntityState.Modified;
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(comment);
        }
        // GET: Blog/Comments/Delete/5
        public ActionResult Delete(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Comment comment = db.Comments.Find(id);
            if (comment == null)
            {
                return HttpNotFound();
            }
            return View(comment);
        }
        // POST: Blog/Comments/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Comment comment = db.Comments.Find(id);
            db.Comments.Remove(comment);
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
}
```
Scaffolding: <br />
![Scaffolding](/images/Story2_Scaffolding.png)

Back to: [Assignments](#my-assignments)
<br />

### 3. Create and Implement Partial View for Blog Posts
Following creation of the comment model and CRUD pages, I was assigned to create a partial view for displaying comments accross other pages so that they weren't only regulated to being displayed on the Comments Index page. I did this by replacing the table on the comments Index page with a method that calls the Comments.cshtml partial view.
<br />
Before:
```c#
//Index.cshtml
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>

@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Index</h2>

@Html.Partial("_Comments")

@*<p>
        @Html.ActionLink("Create New", "Create")
 </p>

    @Html.Partial("~/Areas/Blog/Views/Comments/_Comments.cshtml", Model*@)

Before:
//_Comments.cshtml
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>

@Html.ActionLink("Create New", "Create")

<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Message)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.CommentDate)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Likes)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Dislikes)
        </th>
        <th></th>
    </tr>
    @foreach (var item in Model)
    {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Message)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.CommentDate)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Likes)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Dislikes)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.CommentId }) |
                @Html.ActionLink("Details", "Details", new { id = item.CommentId }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.CommentId })
            </td>
        </tr>
    }
```
After:
```c#
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" />
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.7.0/css/bootstrap.min.css">
<link rel="stylesheet" href="~/Content/Areas/Blog.css">

<div class="comment-index--comments_section">
    <h3>Comments</h3>
    <hr />

    <div class="comment-index--container">
        <div class="comment-index--row">
            <div class="col-md-12">

                <a href="@Url.Action("Create")" class="btn" style="background-color: #d6972a; color: black; font-weight: bold;">Create New Comment</a>

                @foreach (var item in Model)
                {
                    <div class="comment-index--comment_item">
                        <img src="https://i.imgur.com/yTFUilP.jpg" alt="User Avatar" class="comment-index--user_avatar rounded-circle">
                        <div class="comment-index--comment_content">
                            <h5 class="mt-0">@item.Author</h5>
                            <span class="text-muted">@item.CommentDate.ToString("MMM dd, yyyy")</span>
                            <p>@Html.DisplayFor(modelItem => item.Message)</p>
                            <div class="comment-actions">
                                <button class="btn btn-link"><i class="fa-regular fa-thumbs-up" style="color: #f04d44;"></i> <span class="comment-index--like_count">@item.Likes</span></button>
                                <button class="btn btn-link"><i class="fa-regular fa-thumbs-down" style="color: #bd1a11;"></i> <span class="comment-index--dislike_count">@item.Dislikes</span></button>
                                <button class="btn btn-link"><i class="fa-solid fa-reply"></i> Reply</button>
                                @if (User.IsInRole("Admin"))
                                {
                                    @Html.ActionLink("Details", "Details", new { id = item.CommentId }, new { @class = "btn btn-link" })
                                    @Html.ActionLink("Edit", "Edit", new { id = item.CommentId }, new { @class = "btn btn-link" })
                                    <button class="btn btn-link"><i class="fas fa-trash-alt trash-icon"></i></button>
                                }
                            </div>
                        </div>
                    </div>
                }
            </div>
        </div>
    </div>
</div>
```
Back to: [Assignments](#my-assignments)
<br />

### 4. Style the Comment Section
The next assignment was to make the comment section look more like a comment section you would find on any popular website. This invovled a redesign of how the author, datetime, message, and buttons were displayed. Some of the framework for that code is seen above. It also involved creating a way for the admins to inspect, edit, and delete comments and set everthing up for future functionality implementation. To do this, I modified the code in the Comments.cshtml file, then created, linked, and edited a Blog.css file with the appropriate naming conventions required for project management. 

```css
/* BLOG AREA STYLING */
/* CSS color variables */
/*:root { /* Color palette for css */
/* --main-color: #BD1A11;  Red */
/*--main-color--light: #F04D44;*/ /* Red, a shade lighter */
/*--secondary-color: #D6972A;*/ /* Yellow gold */
/*--light-color: #FFFBFB;*/ /* White */
/*--dark-color: #000000;*/ /* Black */
/*--secondary-color--dark: #9D7C39;*/ /* Dark Gold */
/*}*/

/* Start _Comments.cshtml section */
comment-index--comment_item {
    display: flex;
    align-items: flex-start;
    margin-bottom: 20px;
}

.comment-index--comment_item .comment-index--user_avatar {
    flex-shrink: 0;
    width: 40px;
    height: 40px;
    margin-right: 10px;
}

.comment-index--comment_item .comment-index--comment_content {
    flex-grow: 1;
}

.comment-index--like_count {
    color: var(--main-color--light);
}

.comment-index--dislike_count {
    color: var(--main-color);
}

.comment-index--comment_item {
    background-color: var(--light-color);
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
    color: black;
    max-width: 500px;
    margin-left: auto;
    margin-right: auto;
    box-shadow: 5px 5px 5px var(--main-color);
}
/* End _Comments.cshtml section */
```

Back to: [Assignments](#my-assignments)
<br />

### 5. Implementing Comment Feature Functionality
The next task was to implement the Upvote/Downvote functionality by incrementing the Like and Dislike properties by 1 when a user clicks the corresponding buttons. I had to teach myself Ajax and JSON to create an asynchronously updating property so that the page didn't reload when the buttons were clicked. This was a very tough challenge and involved making changes accross multiple files including but not limited to Comments.cshtml, Blog.js, Index.cshtml, and CommentsController.cs. I attached event handlers to the button classes that were used to make Ajax requests to the server and implemented exceptions to handle any errors that might occur during calls. With my code completed, the like and dislike buttons updated the respective counts asynchronously without reloading the page when clicked.

```c#
//...
// like and dislike methods
        [HttpPost]
        public JsonResult AddLike(int id)
        {
            var comment = db.Comments.Find(id);
            if (comment == null)
            {
                return Json(new { error = "Comment not found." });
            }

            comment.Likes += 1;
            db.Entry(comment).State = EntityState.Modified;
            db.SaveChanges();

            var result = new JsonResult();
            result.Data = new { likes = comment.Likes };
            return result;
        }

        [HttpPost]
        public JsonResult AddDislike(int id)
        {
            var comment = db.Comments.Find(id);
            if (comment == null)
            {
                return Json(new { error = "Comment not found." });
            }

            comment.Dislikes += 1;
            db.Entry(comment).State = EntityState.Modified;
            db.SaveChanges();

            var result = new JsonResult();
            result.Data = new { dislikes = comment.Dislikes };
            return result;
        }
//...
```

```javascript
//Blog.js
//Like button
function Like(id) {
    $.ajax({
        url: '/Comments/AddLike',
        type: 'POST',
        data: { id: id },
        success: function (response) {
            $('#like-count-' + id).text(response.likes);
        },
        error: function (xhr, status, error) {
            console.error(error);
        }
    });
}

//Dislike button
function Dislike(id) {
    $.ajax({
        url: '/Comments/AddDislike',
        type: 'POST',
        data: { id: id },
        success: function (response) {
            $('#dislike-count-' + id).text(response.dislikes);
        },
        error: function (xhr, status, error) {
            console.error(error);
        }
    });
}
```

```c#
//_Comments.cshtml
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" />
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.7.0/css/bootstrap.min.css">
<link rel="stylesheet" href="~/Content/Areas/Blog.css">

<div class="comment-index--comments_section">
    <h3>Comments</h3>
    <hr />
    <div class="comment-index--container">
        <div class="comment-index--row">
            <div class="col-md-12">
                <a href="@Url.Action("Create")" class="btn" style="background-color: #d6972a; color: black; font-weight: bold;">Create New Comment</a>
                @foreach (var item in Model)
                {
                    <div class="comment-index--comment_item">
                        <img src="https://i.imgur.com/yTFUilP.jpg" alt="User Avatar" class="comment-index--user_avatar rounded-circle">
                        <div class="comment-index--comment_content">
                            <h5 class="mt-0">@item.Author</h5>
                            <span class="text-muted">@item.CommentDate.ToString("MMM dd, yyyy")</span>
                            <p>@Html.DisplayFor(modelItem => item.Message)</p>
                            <div class="comment-actions">
                                <button onclick="Like(@item.CommentId)">
                                    <i class="fa-regular fa-thumbs-up" style="color: #f04d44;"></i>
                                    <span class="comment-index--like_count" id="like-count-@item.CommentId">@item.Likes</span>
                                </button>
                                <button onclick="Dislike(@item.CommentId)">
                                    <i class="fa-regular fa-thumbs-down" style="color: #bd1a11;"></i>
                                    <span class="comment-index--dislike_count" id="dislike-count-@item.CommentId">@item.Dislikes</span>
                                </button>
                        <button class="btn btn-link"><i class="fa-solid fa-reply"></i> Reply</button>
                        @if (User.IsInRole("Admin"))
                        {
                            @Html.ActionLink("Details", "Details", new { id = item.CommentId }, new { @class = "btn btn-link" })
                            @Html.ActionLink("Edit", "Edit", new { id = item.CommentId }, new { @class = "btn btn-link" })
                            <button class="btn btn-link"><i class="fas fa-trash-alt trash-icon"></i></button>
                        }
                         </div>
                     </div>
                 </div>
                }
            </div>
        </div>
    </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="~/Scripts/Areas/Blog.js"></script>
```
Back to: [Assignments](#my-assignments)
<br />

### 6. Create A Like Ratio Progress Bar
My final story was to create a bootstrap and Ajax progress bar for each Comment that shows a visual representation of the percentage of Likes/Dislikes it currently has. I used the LikeRatio() method to get the percentage to then dynamically fill in the progress bar and used alerts to check for correct returns. I implemented Ajax to update the progress bar when a Comment is Liked or Disliked without having to reload the browser/web page. This was another fun and challenging story that tested my full range of skills.

![Progress Bar](/images/Story6_ratiobar.gif)

Index.cshtml:
```c#
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>
@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}
<h2>Index</h2>
@{
    Func<int, int, double> LikeRatio = (likes, dislikes) =>
    {
        int totalUserVotes = likes + dislikes;

        if (totalUserVotes == 0)
        {
            return 0;
        }
        else
        {
            double ratioOfVotes = (double)likes / totalUserVotes * 100;
            return Math.Round(ratioOfVotes, 2);
        }
    };
}
@Html.Partial("_Comments", Model, new ViewDataDictionary { { "LikeRatio", LikeRatio } })
```

Comments Controller:
```c#
namespace TheatreCMS3.Areas.Blog.Controllers
{
    public class CommentsController : Controller
    {
        private ApplicationDbContext db = new ApplicationDbContext();

        // defines the LikeRatio() method for controller
        private double LikeRatio(int likes, int dislikes)
        {
            int totalUserVotes = likes + dislikes;
            
            if (totalUserVotes == 0)
            {
                return 0;
            }
            else
            {
                double ratioOfVotes = (double)likes / totalUserVotes * 100;
                return Math.Round(ratioOfVotes, 2);
            }
        }
        // GET: Blog/Comments/GetLikeRatio
        [HttpGet]
        public ActionResult GetLikeRatio(int commentId)
        {
            var comment = db.Comments.Find(commentId);
            if (comment == null)
            {
                return Json(new { error = "Comment not found." }, JsonRequestBehavior.AllowGet);
            }
            double likeRatio = LikeRatio(comment.Likes, comment.Dislikes);
            return Json(new { likeRatio = likeRatio }, JsonRequestBehavior.AllowGet);
        }
        //GET: Blog/Comments
        public ActionResult Index()
        {
            var userComments = db.Comments.ToList();

            foreach (var comment in userComments)
            {
                comment.LikeRatioValue = comment.LikeRatio();
            }
            return View(userComments);
        }
//...
```

Comment.cs:
```c#
//Creates a model in the Blog area called Comment | model will represent a user's comment on a blog post
namespace TheatreCMS3.Areas.Blog.Models
{
    public class Comment
    {
        //defining the Comment class with appropriate properties and methods
        public int CommentId { get; set; }
        public ApplicationUser Author { get; set; }
        public string Message { get; set; }
        public DateTime CommentDate { get; set; }
        public int Likes { get; set; }
        public int Dislikes { get; set; }
        // defines a property for LikeRatioValue
        public double LikeRatioValue { get; set; }

        public Comment()
        {
            CommentDate = DateTime.Now;
        }
        public double LikeRatio()
        {
            if (Likes + Dislikes == 0)
            {
                return 0;
            }
            return (double)Likes / (Likes + Dislikes) * 100;
        }
    }
}
```

Blog.js:
```js
// Function to update the progress bar
function updateProgressBars() {
    $('.comment-index--bg-custom').each(function () {
        var progressBar = $(this);
        var commentId = progressBar.data('comment-id');

        $.ajax({
            url: '/Blog/Comments/GetLikeRatio',
            data: { commentId: commentId },
            success: function (response) {
                var likeRatio = response.likeRatio;
                var dislikesRatio = 100 - likeRatio;

                progressBar.css('background-image', 'linear-gradient(to right, #D6972A ' + likeRatio + '%, #D6972A ' + likeRatio + '%, #28A745 ' + likeRatio + '%, #28A745 ' + likeRatio + '%)');
                progressBar.find('.likes-count').text(likeRatio.toFixed(2) + '%');
                progressBar.find('.dislikes-count').text(dislikesRatio.toFixed(2) + '%');
            },
            error: function () {
                console.log('Error occurred while updating progress bar.');
            }
        });
    });
}
// Calls the updateProgressBars function initially
updateProgressBars();
```
_Comments.cshtml_:
```c#
@model IEnumerable<TheatreCMS3.Areas.Blog.Models.Comment>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" />
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.7.0/css/bootstrap.min.css">
<link rel="stylesheet" href="~/Content/Areas/Blog.css">

<div class="comment-index--comments_section">
    <h3>Comments</h3>
    <hr />
    <div class="comment-index--container">
        <div class="comment-index--row">
            <div class="col-md-12">
                <a href="@Url.Action("Create")" class="btn" style="background-color: #d6972a; color: black; font-weight: bold;">Create New Comment</a>
                @foreach (var item in Model)
                {
            <div class="comment-index--comment_item">
                <img src="https://i.imgur.com/yTFUilP.jpg" alt="User Avatar" class="comment-index--user_avatar rounded-circle">
                <div class="comment-index--comment_content">
                    <h5 class="mt-0">@item.Author</h5>
                    <span class="text-muted">@item.CommentDate.ToString("MMM dd, yyyy")</span>
                    <p>@Html.DisplayFor(modelItem => item.Message)</p>
                    <div class="comment-actions">
                        <button onclick="Like(@item.CommentId)">
                            <i class="fa-regular fa-thumbs-up" style="color: #f04d44;"></i>
                            <span class="comment-index--like_count" id="like-count-@item.CommentId">@item.Likes</span>
                        </button>
                        <button onclick="Dislike(@item.CommentId)">
                            <i class="fa-regular fa-thumbs-down" style="color: #bd1a11;"></i>
                            <span class="comment-index--dislike_count" id="dislike-count-@item.CommentId">@item.Dislikes</span>
                        </button>
                        <button class="btn btn-link"><i class="fa-solid fa-reply"></i> Reply</button>
                        @if (User.IsInRole("Admin"))
                        {
                            @Html.ActionLink("Details", "Details", new { id = item.CommentId }, new { @class = "btn btn-link" })
                            @Html.ActionLink("Edit", "Edit", new { id = item.CommentId }, new { @class = "btn btn-link" })
                            <button class="btn btn-link"><i class="fas fa-trash-alt trash-icon"></i></button>
                        }
                    </div>
                </div>
                <style>
                    .comment-index--bg-custom {
                        background-image: linear-gradient(to right, #D6972A @(item.LikeRatioValue)%, #D6972A @(item.LikeRatioValue)%, #28A745 @(item.LikeRatioValue)%, #28A745 @(item.LikeRatioValue)%);
                    }
                </style>
                <div class="progress-bar comment-index--bg-custom" id="progress-bar-@item.CommentId" data-comment-id="@item.CommentId" role="progressbar">
                    <span class="progress-text" style="color: #000000; font-weight: 600;">
                        <span class="likes-count"></span> Likes | <span class="dislikes-count"></span> Dislikes
                    </span>
                </div>
            </div>
                }
            </div>
        </div>
    </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="~/Scripts/Areas/Blog.js"></script>
```

Back to: [Assignments](#my-assignments)

<hr><hr>

## _Learning Hightlights_

* Used Agile and Scrum methodoligies and worked within Azure DevOps, utilizing boards, wikis, repos, and stories.
* Learned how to manage multiple drafts of my work and ask meaningful relevant question during the coding process to ensure quality and correct implementation of project guidlines.
* Gained experience with Git in Visual Studio, version control branching, console command, merge conflict resolution, and reverting to previous points in my work.
* Learned to make small, concise commits that were easy for the project manager to handle, and how to review the history of the project and recover code when needed.
* Worked with a team on a refactored development project and learned how to focus on one task at time while maintaining communication with the team and project managers through daily standups.
* Improved skills and confidence when working with C#, Git, Razor, Ajax, JSON, jQuery, JavaScript, Bootstrap, IDEs, and .NET.
* It was so fun to finally put everything I learned into practice and I had a fabulous time doing it.

Back to: [Top](#tta-live-project)

<hr>
<br /><br /><br /><br />
Author:
Viktoriya Furlow - Actual Wizard

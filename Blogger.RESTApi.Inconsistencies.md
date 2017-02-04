<!--
PostId: 5013152054793843236
Title    : Blogger REST Api inconsistencies
Labels   : blogger, REST, api
Format	 : markdown
Published: true
-->

Yesterday I noticed that blogger API now supports posting as draft. This wasn't the case when I wrote [easyblogger](https://github.com/raghur/easyblogger) - a cli tool to edit and post to Blogger from Vim.

In any case, since the API now has the ability, I updated EasyBlogger to post
as draft. Trouble came when I went to update an existing draft - it fails with
a 404

```
C:\Users\raghuramanr>easyblogger file c:\Users\raghuramanr\blogentries\a.sample.draft.md
....
....
    raise HttpError(resp, content, uri=self.uri)
googleapiclient.errors.HttpError: <HttpError 404 when requesting https://www.googleapis.com/blogger/v3/blogs/7642453/posts/1132458907272271320?alt=json&revert=false returned "Not Found">
```

Trying to get a post saved as draft fails - apparently, one needs to pass in
`view="AUTHOR"` to get drafts. fair enough..

But my first reaction? Sheer happiness! There's hope!!! If Google can screw up
REST API consistency and principle of least surprise, there's hope for us mere
mortals who write APIs at work in enterprisey situations that're probably going
to be consumed by one or more internal teams at most.

Anyway, here's how I've patched it together but publishing it first, then
editing it and then reverting it to a draft if needed. Yuck!

```
       postStatus = service.posts().get(
            blogId=self.blogId,
            postId=postId,
            view="AUTHOR",
            fields="status"
        ).execute()['status']
        mustRevert = postStatus != 'DRAFT' and isDraft
        mustPublish = postStatus == 'DRAFT' and not isDraft
        if postStatus == "DRAFT":
            service.posts().publish(
                blogId=self.blogId,
                postId=postId
            ).execute()
        resp = service.posts().patch(
            blogId=self.blogId,
            postId=postId,
            body=blogPost,
            revert=mustRevert,
            publish=mustPublish).execute()
        return resp
```


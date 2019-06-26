---
title: Image Browser
page_title: Editor Image Browser | Kendo UI Editor HtmlHelper for ASP.NET Core
description: "Learn about the Image Browser component of the Kendo UI Editor HtmlHelper for ASP.NET Core (MVC 6 or ASP.NET Core MVC)."
slug: htmlhelpers_editor_image_browser_aspnetcore
position: 6
---

# Image Browser

By default, the `Insert Image` tool opens a simple dialog that allows you to type in or paste the URL of an image and, optionally, specify a tooltip.

The Editor also supports another way of picking an image by browsing a list of predefined files and directories. Uploading new images is also supported. For a runnable example, refer to the [demo on file and image browser in the Editor](https://demos.telerik.com/aspnet-core/editor/imagebrowser).

To retrieve and upload the files and directories, the image browser needs a server-side implementation. To configure the image browser tool, use the [`ImageBrowser()`](/api/Kendo.Mvc.UI.Fluent/EditorBuilder#imagebrowsersystemactionkendomvcuifluenteditorimagebrowsersettingsbuilder) method.

```
@(Html.Kendo().Editor()
    .Name("editor")
    .ImageBrowser(imageBrowser => imageBrowser
        .Image("~/Content/UserFiles/Images/{0}")
        .Read("Read", "ImageBrowser")
        .Create("Create", "ImageBrowser")
        .Destroy("Destroy", "ImageBrowser")
        .Upload("Upload", "ImageBrowser")
        .Thumbnail("Thumbnail", "ImageBrowser")
    )
)
```

The following list provides information about the default requests and responses for the `Create()`, `Read()`, `Destroy()`, and `Upload()` operations.

- `Create()`&mdash;Makes a `POST` request for the creation of a directory with the following parameters and does not expect a response.

        { "name": "New folder name", "type": "d", "path": "foo/" }

- `Read()`&mdash;Makes a `POST` request that contains the `path` parameter which specifies the path that is browsed and expects a file listing in the following format.

    ```
    [
        { "name": "foo.png", "type": "f", "size": 73289 },
        { "name": "bar.jpg", "type": "f", "size": 15289 },
        ...
    ]
    ```

    `name` from the previous example is the name of the file or directory, `type` is either an `f` for a file or a `d` for a directory, and `size` is the file size (optional).

- `Destroy()`&mdash;Makes a `POST` request with the following parameters:

    - `name`&mdash;The file or the directory that will be deleted.
    - `path`&mdash;The directory in which the file or the directory resides.
    - `type`&mdash;The file or the directory that will be deleted (an `f` or a `d`).
    - `size`&mdash;(Optional) The file size, as provided by the `Read()` response.

- `Upload()`&mdash;Makes a `POST` request. The request includes `FormData` which contains the upload path and the file name and type. Its payload consists of the uploaded file. The expected response is a `file` object in the following format:

    ```
    { "name": "foo.png", "type": "f", "size": 12345 }
    ```

- `Thumbnail()`&mdash;Makes a `GET` request for each individual image to display its thumbnail in the explorer window. The single request parameter is the `path` to the image. The service is expected to respond with the image file for the thumbnail.

- `Image()`&mdash;Used by the Editor to generate the `src` attribute of the original image when the image is inserted. It results in a `GET` request generated by the browser for each inserted image. The URL can point to a file system or to a service and is parameterized&mdash;the `{0}` placeholder denotes the `path` and `fileName` as received from the `Read()` service.

You can update all of these requests and responses through the [`ImageBrowser()`](/api/Kendo.Mvc.UI.Fluent/EditorBuilder#imagebrowsersystemactionkendomvcuifluenteditorimagebrowsersettingsbuilder) configuration. Each of them exposes more options that you can tweak.

## See Also

* [File and Image Browser by the Editor HtmlHelper for ASP.NET Core (Demo)](https://demos.telerik.com/aspnet-core/editor/imagebrowser)
* [JavaScript API Reference of the Editor](http://docs.telerik.com/kendo-ui/api/javascript/ui/editor)
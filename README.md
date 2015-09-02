# <img src="https://raw.githubusercontent.com/norganna/asciid-usermacro/master/asciid.png" title="asciid-usermacro" height="30px"> AsciiDoctor User Macro for Confluence.
User macro to allow GitHub flavoured markdown blocks inside Confluence pages via asciidoctor.js.

## Description
This macro is designed to render the AsciiDoc body content in the page whilst preserving the original markup code in the macro body, so that consequential edits of the page can update the original code.
The AsciiDoc code is rendered at page display time by the [asciidoctor.js](https://github.com/asciidoctor/asciidoctor.js) javascript library.

## Installation
Add the following to your user macros page, which you can find by:
* going to the **Configuration** page as an administrator 
* selecting **User Macros** in the left navigation panel (in the **Configuration** block)
* navigating to **Create a User Macro** page
* entering the **Information** and **Definition** as per the comments at the top of the following code
* pasting the code into the **Template** text box

## Code
```
## Macro name: asciid
## Macro title: AsciiDoctor markup content
## Categories: Formatting
## Description: Display content in AsciiDoc format via the asciidoctor.js engine.
## Visibility: to all users
## Icon URL: https://raw.githubusercontent.com/norganna/asciid-usermacro/master/asciid.png
## Documentation URL: http://asciidoctor.org/docs/asciidoc-syntax-quick-reference/
## Macro has a body: Y
## Body processing: Unrendered
## Generates: HTML
##
## @noparams
##
## Developed by: Ken Allan (https://github.com/norganna)
## Date created: 2015-09-02

#set ($randomId="")
#macro(getRandomId)
#set ($string="")
#set ($rand=$string.class.forName("java.util.Random").getConstructor().newInstance())
#set ($randomId=$rand.nextInt(2147483647).toString())
#end
#getRandomId()

<div id="asciidcontent_$randomId" class="asciid">
</div>
<link type="text/css" rel="stylesheet" href="//www.norganna.com/cdn/css/asciid.css" media="all">
<script src="//www.norganna.com/cdn/js/asciidoctor.js/dist/asciidoctor-all.js">
</script>
<script src="//www.norganna.com/cdn/js/asciidoctor.js/dist/asciidoctor-docbook.js">
</script>
<script>
jQuery(function() {
    var body = $jsonator.convert($body).serialize(),
        options = Opal.hash2(
            ['doctype', 'backend', 'attributes'],
            {
                doctype: 'article',
                backend: 'html5',
                attributes: ['showtitle']
            }
        ),
        output = Opal.Asciidoctor.$convert(body, options);

    jQuery('#asciidcontent_$randomId').html(output);
});
</script>
```

## Usage
This code is safe to include multiple times per page.
Include on your confluence page by using the `{asciid}` tag like this:

```markdown
{asciid}
= Hello, AsciiDoc!
Doc Writer <doc@example.com>

An introduction to http://asciidoc.org[AsciiDoc].

 
== Header 2
 
Content content.

* List item 1
* List item 2
{asciid}
```

## License

Copyright 2015, Ken Allan, MIT License.

See LICENSE file for more information.

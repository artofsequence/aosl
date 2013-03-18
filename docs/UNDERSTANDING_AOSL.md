= Understanding AOSL

This document is an introduction to the way the AOSL format works, how to read and write it,
written in a more human-readable prose than the official specification document.
It's a technical overview of the format.

This document is a must-read for developers making tools and players
using the AOSL format. It can also be interesting for other people involved
with the format, to understand what it does exactly, what it's good at or
bad at.

But before diving into the gory details of the format itself, some
rather philosophical concerns have to be detailed to explain why it's organised the
way it is.

If you can still directly jump to the technical description starting with [the general
organisation of the format].

== Objectives Overview

AOSL is, to make it short and simple, a **universal format to describe digital stories**.
In our context, a digital story is a sequence of "stuffs" happening on the display and
audio output of a digital support, anything really. There is a detailed explaination
about digital story-telling [on the website](http://artofsequence.org).

One of the important constraint we impose to the format is that the story is not stuctured
around time. When you read a book or a comic, you follow a sequence of events happening in
 your mental theater. The way you ingest the story is independent from time, you go at your
 rythm. In particular in comics, as ScottNcLoud note it, time is suggested through space (in
 a pannel, a page, illustration etc...). However, a time structure is suggested using spatial
 structure in a comics or even length of texts. (read "Understanding Comics" and "Making Comics"
 from Scott McLoud for more details and examples about this).
Therefore, AOSL make the author segment his story in a way similar to comics.
As ["Turbo Media"] authors and other digital story-telling experiments show, it's one of
the particularities of digital platforms -being able to change things perceived "on the fly"- which
also gives a lot of narrative power.

One objective of AOSL is to not get in the way of people willing to explore possibilities enabled
by digital plateforms, like having branches in a story, having repeating (looping) parts of a story,
animated transitions, etc. Such features are not to be taken lightly as it is really easy to lose the
reader with fancy animations or game-like choices which makes reading hard - breaking the suspension of 
disbelief. However, some experimentations shows that it is possible, and incredibly efficient to use such 
narrative tools to enhance the reader's experience. With this in mind, AOSL is totally designed to keep digital platforms specificities available for authors.

Another objective of AOSL is to impose no limitation on the kind of content that is used by the author.
Theorically, it can be any kind of media- images, audio, video, interractive software; 
with no format limitation.
However, for practical (and realistic) purpose, the players implementations will impose some limitations
at least on the media formats they can handle. The important thing to understand is that AOSL is supposed
to be generic enough so that player-specific implementation will just have to add constraints on formats
but nothing else, to be able to play a story.

As AOSL isn't structured around time, the sequence the reader will see will be read like turning pages
of a comics or reading a PowerPoint presentation. 
However, as videos and interractive media can be used in AOSL, it also means that one stage of the story
can have media which is based on time. It means that for example there can be a little video playing
in the tv screen of a character which we are the reading the story from. However the story itself can never
be blocked by the playing of a video or anything based on time. The reader can always go to the next
or previous stage, interrupting whatever is playing at a specific stage.

All these features and constraints are meant to make AOSL a universal way of describing a story which
exloit the digital nature of the platforms it's read on. It is not platform-specific so nothing
other than the lack of tools prevents the author to publish his work on several different platforms,
as long as they are digital.

Also, AOSL is designed for tools, not for humans. It describe the story in a way that is easily
understandable for tools. It is not supposed to be written at all by hand, like a classic programming
language.
It's also designed as a meta-format because of the lack of restrictions on the content.
One use of this format is to convert the story defined in AOSL to another more specific format,
maybe a compressed binary one targetted at a player running on an embedded platform.
AOSL is not meant as a primary usage to be read and interpreted directly. 
But as [the web player demo shows](http://demo.artofsequence.org) shows,
 it don't forbid it either. Interpreting directly AOSL data is one possible and simple way of working with it.
 If for some reasons the interpreter needs the data to be different (faster to read for example) then
 it is easy to build a convertion tool from AOSL to another format.

A more complete and detailed explaination of the design of AOSL is available [in this document].


=== XAOSL

XAOSL is basically AOSL in a zip.

AOSL describe how  to manipulate some media resources to tell the story, 
but it just refer to the resources using URIs. Like HTML, it don't really embedd the media
files into itself, but just provide adresses for the tool or interpreter to retrieve
the media data and use it once ready.

XAOSL is an archive file (a zip file) which contain both AOSL files
and the media files these AOSL files refer to, plus some meta data files (like author infos). 
While AOSL is modeled as a kind of HTML for digital story-telling, 
XAOSL is more like the ePub format, even if it differ widely on the
purpose and philosophy.

It also impose some restrictions on the possible media formats it can contain.
This mean that if several different players can read XAOSL, then a story in XAOSL format
will be readable on all these players.
Therefore, XAOSL is really a universal file format for digital stories, usable as is by players.

XAOSL is specified in details in [another document] and there is also [an introduction document] to it.

== Design 

Now that we have some idea of what this format is supposed to do, let's see how AOSL is designed
to solve our problems.

If you read the documentation available, you will ofen find that the AOSL format is defined as "describing" 
a digital story. It is, actually, a useful lie (as Scott Meyer would have put it).

In reality, AOSL doesn't really statically describe the story by specifying where things goes or when.
That's what HTML or ePub does because they are describing /documents/.

AOSL, instead, tell the interpretter **how to generate the story**. This is an important difference because
it means that the story is built "on the fly" by the computer, via the intrepreter. 
Often when I explain this point to other developers they reply, naturally, by asking but why not do it
 in a simpler or more obvious way, like HTML, by just describing things on some kind of "timeline". 
This is a legitimate question but it also point a lack of understanding of 
the procedural nature of digital platforms.

AOSL is designed this way to enable authors to use the procedural properties of digital platforms 
to tell their stories.
To put it another way, AOSL format support the same possibilities than a general purpose programming language.
But as it is designed for tools (used by authors), it don't force authors to have to 
get some programming skills to be able to use the digital platform fully. Instead, it is designed so that,
by generating the story procedurally, authors can experiment with the possibilities offered by having
a story on a digital plateform.

Procedurally generating the story is what allow branching the story
(having a condition that makes the story goes one way or the other),
either by giving choice to the reader or by using other kind of condition to make this choice,
 or even a random choice not even visible until the reader get back and forth in the story.
That's also what allows having some part of the story repeat (looping) whithout
the author having to repeat that part explicitely, or having a hard time having to retouch everything
each time she wants to change something in the loop. It also allow some new and unexplored ways of 
telling stories, being interactive or not.

AOSL focus on describing the changes applied to media, happening in each transition between the stages of
the story. Inside an AOSL file, technically, the story is totally described by a cyclick oriented 
graph of transitions, each transition being a set of changes to apply to the media in the canvas.

To be easy to manipulate by tools (inter-operability), AOSL is defined as a XML-based format.
To be clear, AOSL could have been defined using any language that would support DOM.
So we assume that in the future some people will use JSON or other languages instead of XML.
However, our specification is based on XML because of it's strongly typed nature which help a lot 
defining the specificities of the format.

With all that in mind, it's now time to get into the technical details.

== General Organisation

AOSL is an XML-based format.
For version 1.0 of the language, the XML namespace for all AOSL tags is :

    artofsequence.org/aosl/1.0

Most of the time we use the .aosl suffix for AOSL files, but this is not a requirement.
This namespace is also a public adress where you can get the xsd specification file.

The root element of AOSL is the **sequence**. 
A sequence must contain:
 - a media **library**;
 - a **canvas** in which the story happen;
 - a **story** which describe how to generat the story;

It can optionally contain some **meta** information about the sequence 
but we will not talk about this part in this document.

Obviously, some required attributes and elements are necessary to write a well-formed AOSL file. 
Here is what a valid AOSL file but with an empty story looks like:

    <?xml version="1.0" encoding="utf-8" ?>

    <sequence xmlns="artofsequence.org/aosl/1.0" 
        id="some-id" 
        name="Some Title" >
    
        <library>
        </library>
    
        <canvas>
            <area x="800" y="600" z="0" />
        </canvas>
    
        <story begin="stage_0" >
            <navigation></navigation>
            <moves></moves>
            <stages>
                <stage id="stage_0" />
            </stages>
        </story>
    
    </sequence>



In this example, we implicitely use the AOSL namespace.

Now let's take a quick look at each important element we have here:

 * `<sequence>` : the root element - we will get back to it soon.
 * `<library>` : the **library** will contain **resources** that hold info about the media that are needed 
 to read this story.
 * `<canvas>` : the **canvas** define the initial state of **objects** which compose our story and describe the graphic area.
 * `<story>` : the **story** contain **changes** which are to be applied to **objects** between each **stage** of the sequence, and also define how the reader will **navigate**.

All those elements are required to write a valid AOSL sequence.

== Hello World

Starting from this empty story we will try to make a simple Hello World story to understand
how each part works. First, take a look [this page] to see the final result we are trying to produce here.

We assume that the AOSL file is in a directory containing a 'media' directory. The images we will use available [there] and we will assume they are in this directory.

=== Sequence

Before building our story, we will title it.
The `<sequence>` contain the whole story and provide some information we will need to fill,
by setting it's attributes:

 - `name`: the title of the story, used by players in menus to choose the sequence, and sometimes also displayed while the sequence is running;
 - `id`: this is the easy-to-parse unique identifier of the sequence which can be used by the tools and interpreters instead of the title. The title can contain non-alphanumeric characters but the identifier must not, which help a lot when programming tools for it;

Back to our "hello world", we write:

    <sequence xmlns="artofsequence.org/aosl/1.0" 
        id="hello-world" 
        name="Hello, World!" >
    

=== Library

The first thing we need to do is to specify which external files we will need to read the story.
In our case we only need the image files contained in the 'media' directory.
However in a more complex story it could be audio files or video files or any other kind of media.

Note that an author using an edition tool like AOS Designer would not start by setting up the library first
but would first try to draft some kind of story-board using abstract colored shapes instead of the final
images for example. In our current Hello World example we just focus on how each part of the AOSL format
works so we have a different approach, more targetted at people implementing applications that will read the 
format.

The library, using the tag `<lirbary>`, will contain the list of URIs referencing all media we need to 
telle this story. The interpreter then have a full index of files to load. The interpreter can either
load everything before beginning to play the story, or can first load the media used in the few first stages of
the story and continue loading the rest while the reader is reading. AOSL don't specify how the 
interpreter should load media because loading up-front or streaming can both be good solutions to widely
different contexts, like if a sequence is too long and big it is better to stream, but if it's very short
it's better to load everything first. 

Back to our XML, we add two images to our library so that we can use them in the story:


    <library>
        <resource id="img_hello" type="image" >./media/hello.png</resource>
    </library>

The `<resource>` tag is a reference to one media resource. It associates an id, which
is used into the current sequence, to an URI address specified in it's content part.
The type gives guideline to the interpreter implementation on how to manipulate the media
once loaded. For example, whatever the format, images are all static graphic content, while
audio files woul be only streamed audio data. The URI could also be some stramable video
but the interpreter have to know it is a video to be ready to read it.
It also helps the interpreter to put place-holders instead of the real media in case it
is not loaded fast enough. For example it can set colored rectangles instead of images
that are not ready yet to be displayed. It is not a requirements though.

In our "hello world" story, we want to use two images:


    <library>
        <resource id="img_hello" type="image" >./media/hello.png</resource>
        <resource id="img_world" type="image" >./media/world.png</resource>
    </library>

That's all needed for our case. The library can also refer to another library,
which is useful in case we have several different sequences sharing the same media,
but our "hello world" and most stories will not use this advanced feature.

=== Identifiers and References

It is important to note that, inside an AOSL file, each ids have to be unique.
It simplify how the tools and interpreters will look for elements used in the story.
Unique ids can only contain:

 - characters from a to z and A to Z;
 - numbers (from 0 to 9);
 - characters dot `.` , hyphen `-`, slash `/` and/or underscore `_`;

No any kind of space is allowed, nor any other characters.
Each time you see an 'id' attribute in any element of an AOSL file, it's value 
have to follow exactly these rules.

In an AOSL file, some attributes will refer to ids and they will do it directly,
 by using the id value - for example an object will refer to a media that is used
 by saying "resource='img_hello_world'" in it's attributes.

However, there are other ways to refer to ids, which we call requests.
A request starts with the '#' character. There is no general request specified in AOSL 
because the set of requests possible to use depends on the context.
We will see very soon that, for example, if we want to clear the whole canvas from all
images, we can either hide each object in the canvas one by one, or, more efficiently,
we can just say that we want to hide 'all' the objects. In this case, we will use the
'#all' request. 
As already said, there are other kind of requests available and their use is very specific
to the situation, so we will get into details as needed.

=== Canvas

Now that our images are loaded, we need to setup the graphic context and put these images in.

==== The Graphic Area

First, we will describe the graphic part of the canvas, using the `<area>` tag.
The area just contain spatial attributes, x, y z, which define the graphic canvas, the "box"
in which the story is told.

The z attribute is used only in case of 3D graphics or if some depth management is needed, but most
of the time it can be ommitted, which set it to 0 by default. 

The value of each of these attributes have no specified unit: it can be interpreted as pixels
or as another unit. These attributes are used to give a referential for the rest of the 
graphic objects.
We use the Left-handed coordinate system, which mean that the (0,0,0) coordinate is at
the bottom-left of the display.

Before going further, let's define the graphic area that we will use:

    <canvas>
        <area x="800" y="600" />

        <objects> ... </objects>
    </canvas>

The area we described here have a width of 800 units and a height of 600 units.

[add an image here]

Soon we will describe graphic objects which will have a x and y position inside this range.
Everything outside this range will not be displayed, or will be only partially displayed.

As the units are not specified, the interpreter can decide to interpret the 800 or 600 values 
as pixels, or not. It can also display the graphic area into a 640x480 pixels screen, by applying a 
factor to the objects coordinates. This factor can be deduced by comparing the
display resolution available to the interpreter with the attributes specified in the AOSL graphic
area.

To be clear, the only thing that AOSL require the interpreter to do is to keep the aspect ratio implied
by the area attributes.
The 800x600 graphic area is really just saying that we will use a 4/3 wide graphic area to tell the story.
It could have been written:

    <canvas>
        <area x="4" y="3" />

If it was written like that, then to put a graphic object to the right of the display, we would set it's 
x coordinate to 4. If we wanted it at the top of the display, we would set it's y coordinate to 3.

[add an image here]


In our "hello world" we set x at 800 and y at 600 because it is easier to understand than using very low values.
We can then think like if the display was 800x600 pixels, even if it's not true.
The graphic objects around the top right of the screen would then have x coordinate close to 800 and y coordinate close to 600.

==== Background Color

We also want the background color of the graphic area to be white. We can set it using the "color"
attribute of the canvas:

    <canvas color="white" >
        <area x="800" y="600" />

In each attribute where we can specify a color, we can either use an hexadecimal value prefixed with the
'#' character, like "#ffffff" for white; or use one of the pre-defined names which are the same
than in CSS, like 'white', 'black', 'red' or 'olive'.

=== Objects

To use the media we loaded in the library we need to define objects.

An object is an instance of a media. For example, an image file can contain several smaller images
but we might want to use them separately, like little images cut from one big image. 
In this case, which is common in game and software development, we say that we are using "sprites". 
Sprites are modified instances of an image. They are modified in the sense that we will take only 
a rectangle insside the image, and maybe rotate and make it bigger than the original.
In the same way, in AOSL, an object is a modified instance of a resource.

As there are several types or media resources, there is also several kind of objects. For now we only 
need to use images, so the object type we will use is `<sprite>`. We will need two sprites, one for
each or our images.

    <canvas color="white">
        <area x="800" y="600" />

        <objects>
            <sprite id="hello" resource="img_hello" active="false">
                <graphic></graphic>
            </sprite>
            <sprite id="world" resource="img_world" active="false">
                <graphic></graphic>
            </sprite>
        </objects>

Whatever the type, sprite or another, each object have a set of common attributes:
 - "id": a unique identifier, used later in the story to manipulate the object;
 - "resource": the id of the media resource that this object is an instance of;
 - "active": the initial state of the object;

The state of an object is either active or not-active. This state is interpreted differently depending
on the kind of object. For example, an active sprite is a visible sprite, while a not-active sprite
is hidden, not visible. An audio object would be playing soundor music if it was active and would not be
playing if it was not-active. 
In a generic way, we can say that an active object is perceivable by the reader, while a unactive object just
don't exist to the reader.

Here we have set our two sprites to being non-active initially, which mean we'll have a blank white screen
at the beginning of our story. Almost all changes that will happen while reading the story will just
 turn objects on and/or off, changing canvas content dynamically.

The canvas describe all the objects at once even if they are not used yet. It is designed this way to allow
the interpreter implementation to prepare the objects in the background, even if they are not displayed immediately. The interpreter then don't have to deduce what will be used in the story as everything is stated 
once in the canvas. However, it can't know in advance how the content of the canvas will be used, what
graphic and audio configurations will be reached.

You can see that our sprite objects contain an empty `<graphic>` component. This element contain graphic properties for the object. There are only two kind of set of properties: graphics and streams. 
Graphic properties describe what graphic content to take from the media resource and how to display it
in the graphic area. Streaming properties add informations relative to time, pausing/resuming and length of media which are time-based, like audio, animations and videos. Video objects actually combine both graphic and
stream properties.

Here we just want the sprites to use the default graphic properties so we have set the default values which basically mean "display the whole image without modification". Currently the two sprites's top left
point is positionned at the bottom left point in our display, which is the (0,0) position. 
As we don't want to display these sprites at the beginning, we decide to set their position later, so
for now not specifying the initial position is ok. If a sprite is supposed to be visible from the very
beginning, we would have set an initial position.

There are other advanced features that we will use in a more complexe example, like object groups, but for now these two objects will be enough.

Here is what we have so far:

    <?xml version="1.0" encoding="utf-8" ?>

    <sequence xmlns="artofsequence.org/aosl/1.0" name="Hello World" id="hello-world" >

        <library>
            <resource id="img_hello" type="image" >./media/hello.png</resource>
            <resource id="img_world" type="image" >./media/world.png</resource>
        </library>

      <canvas color="white">
        <area x="800" y="600" />
        <objects>
          <sprite id="hello" resource="img_hello" active="false"> <graphic></graphic> </sprite>
          <sprite id="world" resource="img_world" active="false"> <graphic></graphic> </sprite>
        </objects>
      </canvas>

        <story begin="stage_0" >
            <navigation></navigation>
            <moves></moves>
            <stages>
                <stage id="stage_0" />
            </stages>
        </story>

    </sequence>


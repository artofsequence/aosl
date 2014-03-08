AOSL
====

**Universal Format for Digital Story-Telling**

See Art Of Sequence: http//artofsequence.org

AOSL is a data format to express digital stories. 

It's specified using XML for inter-operability (to help building tools around it) and it can be used either for:

 - direct use: reading and edition of digital media stories directly by interpreting an AOSL file;
 - export: convert the AOSL information to a more efficient format for the target interpreter;
 - create a new file format: you can create new format files including AOSL code to help describing a digital story;

It's not explicitly designed to be read by humans as it's mainly targetted at helping tools to represent and process a digital story. 
It's media-agnostic and interpreter-agnostic.

AOSL describe digital stories, which can exploit the procedural nature of the digital platforms.
It actually don't describe the story, but how to change the objects that compose the story to tell that story.
This imply that:
 - it can use any kind of resources (graphics, audio, video, games) inside the story (limited by what the interpreter can manage);
 - it can generate loops in the story;
 - it can generate branches (choice or other kinds of branching) in the story;
These are the specific differences that difital platforms have compared to other supports.
AOSL allow the user to exploit (with moderation) these specificities.

Another goal of AOSL's design is to allow the author to specify in one place how the content of the story
should be modified depending on the capabilities of the interpreter.

[More documentation can be found on the 'docs' directory.](./docs)

## Derived format: XAOSL

XAOSL is another file format that is based on AOSL but embedd all resources into one archive file. 

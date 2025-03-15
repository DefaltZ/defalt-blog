---
title: 'What is Debbynote'
description: 'describing the process of building Debbynote'
pubDate: 'March 14 2025'

---

### Introduction

Before we learn what is debbynote, its important you all understand the most essence of the most important extracurricular hobby 
that is very close to my heart, which is actually what gave me the idea to come up with this software. 

***Debating***

Most people who will probably be reading this blog (from technical background or from countries/institutions which do not have a 
debating culture) will not understand why does this extracurricular activity need a wholly seperate software for the same. 
To make you understand why, It is important you stop viewing *debating* as just two people arguing against each other in a
private or public setting or even the fucking fiasco that the US presidential debates are. 

<b>What is debating really like?</b>

To folks from south asian economies like India, Bangladesh, Malaysia, Korea etc. Debating is a nearly a full-time endeavour which requires a fuckton of dedication to learn and master. There is several levels of intricacies in debating especially with just how huge some of the tournaments get, even at institutional level. Managing debating tournaments already requires specialised software such as [Tabbycat](https://github.com/TabbycatDebate/tabbycat), visiting the github as linked you can visualise just how huge the contributer and userbase of this software is. There are institutional and national level circuits of debators, equity, CAP, tabbing members who all engage with this activity in different ways and levels. I will not get into too much details about debating right now as we will be diving in anyways when discussing the features of debbynote. Speaking of which, with foundations out of the way let us understand *Debbynote*

### What is Debbynote

![debbynote dark mode](/public/debbynote%20dark%20mode.png)

Notetaking is a very important cornerstone of debating. Tracking opposition arguments, highlighting important points, rebuttals etc. is an important and meticulous task especially for the adjudicators and judges. [Debbynote](https://github.com/DefaltZ/debbynote) caters to people who use digital notes for this process and also makes it easier for people who use paper notes to transition to digital notes with user friendliness and accessibility. 

Debbynote, in its simplest form is a markdown editor with custom extended markdown syntax. It utilises [Tauri](https://v2.tauri.app/), a rust framework and Vanilla JS. Tauri keeps the app lightweight, secure and easily cross-platformable and vanilla JS preserves code-readability (also that I suck at using any other JS frontend framework). 

**Features**

In its prototype phase, it has extended markdown syntax for British Parliamentaries, Asian Parliamentaries and conventional debating formats. Basic tracking such as arguments, rebuttals and worldbuilding can be highlighted with the following syntax 

```
!r -- for rebuttals --
!a -- for arguments --
!wb -- for worldbuilding --
```

For AP and BP debates, custom blockquote highlighting has been implemented that gives the blockquote a preset heading, making debates easier to track. The syntax for which has been highlighted in the github readme linked [here](https://github.com/DefaltZ/debbynote).


**How was this acheived**

To implement the extended markdown syntax, `showdown.js` library was used which is a javascript markdown to html converter, which makes the highlighting thing actually work. Each extension follows this pattern in the `main.js` file:

```js
const customExtension = {
    type: 'lang',
    regex: /!pattern(...)/g,
    replace: '...' // or replace function
}
```
the custom blockquote highlights follow a similar pattern
```js
const og1_highlight = {
    type: 'lang',
    regex: /!og1([\s\S]+?)(?=\n\n|$)/g,
    replace: function(match, content) {
        return `> ##OG 1st speech (PM)\n${content.split('\n').map(line => '> ' + line).join('\n')}`;
    }
};
```
it is rather imperfectly implemented, as the `replace` regex tends to fail with double newlines and inserts blockquotes within other blockquotes, which is an issue I need to fix as of writing this blog. 

All the extensions are then registered with the showdown converter
```js
const converter = new showdown.Converter({
    extensions: [rebutHighlight, arghighlight, /* ... other extensions ... */]
});
```

The extension then works in real time through an event listener which is a default part and parcel of any html based markdown editor
```js
markdownInput.addEventListener('input', updatePreview);
```

This allows debators and adjudicators alike to effectively track everything in a debate with the power of extended markdown, rather than having to drag the cursor along sentences, then choosing a color to highlight the said sentence. Default markdown syntax in itself is not enough to track debates effectively and the average user in the target audience would not be able to remember blockquotes, code highlights etc. 

*The aim of this software is to extend the most basic of markdown syntax as effectively as possible to make it suitable and speedy for average use*

### Goals

The app will also be completely open source with no pricing plans as of now. As of writing this blog, the app is still currently in its alpha phase with a lot of documentation and code rewrite required. The app also needs better UI and UX features. Hell even the option to save files is not implemented yet.

Major future updates will be talked about in this blog.


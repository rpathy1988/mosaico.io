---
layout: post
title:  Email CSS inliners
date:   2016-06-22 07:00:00
categories: email-client-tricks
class: advtable2
draft: yes
---

In order to make emails and email templates compatible with most of the clients and webmails out there, we know that we have to avoid at most everything that is not pure inline css1 style.

To be honest this is true mainly for the two bad guys of email rendering - Outlook and Gmail - but a lot of other webmails and clients have some glitches and behaviour that makes inline style the good choice.

Coding direct inline styles is really an hell of work, and every little adjustment means tons of code, with lot of errors and mistyping.

So here come Email CSS inliners: these lovely pieces of code take your css and html and give you back your "css inlined" email template, ready to be sent.

Simple (ahem...) and effective (ahem...), but how inliners do work and how much are they reliable?

How do I choose my inliner? Many ESP have their native inliners, and some other are available on web or as library.

We tried to analize the most used of them, in order to understand the strategy behind, the weakness and the strenghts.
<!--more-->

The CSS inliners we've tested are:

- [Litmus Putsmail](https://putsmail.com/inliner) 
- [Campaign Monitor](https://inliner.cm/)
- [Mailchimp](http://templates.mailchimp.com/resources/inline-css/)
- [Premailer](http://premailer.dialect.ca/)
- [Zurb](http://foundation.zurb.com/emails/inliner.html)
- [Torchbox](https://inlinestyler.torchbox.com/)
- [MailerMailer](http://www.mailermailer.com/labs/tools/magic-css-inliner-tool.rwp)
- [Juice](http://automattic.github.io/juice/)
- [Styliner](https://github.com/SLaks/Styliner)

Inlining is not a plain and simple procedure: complex css with lot of selectors and rules are not simple to inline, and sometimes are impossible to, so every inliner has its strategy to stick css and html dom together.
Moreover css shorthand and tricks makes the things even more chaotic and unpredictable.

Here is a compact table of the results of test, the table is quite large, so if you prefer, here is the [google sheets version](https://docs.google.com/spreadsheets/d/1B4wR8g5JrmD6lNdoQEtw9Z2DzGkrjxRV536ag21nunE/edit?usp=sharing)

| INLINER                        | Litmus                                           | Campaign Monitor                                        | Mailchimp                                                                                                                             | Premailer                                                               | Zurb                                                                                                                                              | Torchbox                                                                                                                               | MailerMailer                                                                                                                                                                                                                                                     | Juice                                      | Styliner                                                                                                                                                                                             |
|--------------------------------|--------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Type                           | online                                           | online                                                  | online                                                                                                                                | online                                                                  | online                                                                                                                                            | online                                                                                                                                 | online                                                                                                                                                                                                                                                           | library/online                             | library                                                                                                                                                                                              |
| Multiple property declarations | FULL                                             | NONE                                                    | NONE                                                                                                                                  | NONE                                                                    | NONE                                                                                                                                              | NONE                                                                                                                                   | NONE                                                                                                                                                                                                                                                             | NONE                                       | * multiple property occurrences kept only if in the same selector                                                                                                                                    |
| Selector Order                 | FULL                                             | NONE                                                    | NONE                                                                                                                                  | NONE                                                                    | NONE                                                                                                                                              | NONE                                                                                                                                   | NONE                                                                                                                                                                                                                                                             | NONE                                       | FULL                                                                                                                                                                                                 |
| Media Query                    | FULL                                             | * inlining of MEDIA="SCREEN", @media all, @media screen | * inlining of every STYLE MEDIA                                                                                                       | NONE                                                                    | NONE                                                                                                                                              | * inlining of every STYLE MEDIA                                                                                                        | * inlining of MEDIA="SCREEN"                                                                                                                                                                                                                                     | * inlining of every STYLE MEDIA            | * inlining of every STYLE MEDIA                                                                                                                                                                      |
| Value Parsing                  | NONE                                             | * shorthand splitting                                   | NONE                                                                                                                                  | * shorthand validation                                                  | * shorthand validation                                                                                                                            | * shorthand validation                                                                                                                 | NONE                                                                                                                                                                                                                                                             | NONE                                       | FULL                                                                                                                                                                                                 |
| !important Inlining            | FULL - potential Outlook issue                   | FULL - potential Outlook issue                          | NONE                                                                                                                                  | NONE                                                                    | NONE                                                                                                                                              | FULL - potential Outlook issue                                                                                                         | NONE                                                                                                                                                                                                                                                             | NONE                                       | FULL - potential Outlook issue                                                                                                                                                                       |
| Bgcolor Attribute              | NONE                                             | NONE                                                    | NONE                                                                                                                                  | FULL                                                                    | FULL                                                                                                                                              | NONE                                                                                                                                   | NONE                                                                                                                                                                                                                                                             | FULL                                       | NONE                                                                                                                                                                                                 |
| Style Inclusion              | * style with @media and not understood selectors | FULL                                                    | FULL                                                                                                                                  | * style with hover and @font-face. Breaks @keyframes and loose @charset | * style with media using min-width, hover and @font-face. Breaks @keyframes and loose @charset                                                    | NONE                                                                                                                                   | FULL                                                                                                                                                                                                                                                             | * style with @media and font-face          | * style with @media (buggy)                                                                                                                                                                          |
| Pseudoselectors Support        | * :first-child not supported                     | NONE                                                    | FULL                                                                                                                                  | FULL                                                                    | FULL                                                                                                                                              | FULL                                                                                                                                   | * :not() and :empty() not supported                                                                                                                                                                                                                              | FULL                                       | FULL                                                                                                                                                                                                 |
| CSS Inline refactoring         | NONE                                             | FULL                                                    | NONE                                                                                                                                  | FULL                                                                    | FULL                                                                                                                                              | NONE                                                                                                                                   | FULL                                                                                                                                                                                                                                                             | NONE                                       | * strips comments                                                                                                                                                                                    |
| HTML refactoring               | * some code prettify                             | NONE                                                    | NONE                                                                                                                                  | * add or remove some /n                                                 | * add or remove some /n                                                                                                                           | NONE                                                                                                                                   | * some code prettify                                                                                                                                                                                                                                             | NONE                                       | NONE                                                                                                                                                                                                 |


We've checked some behaviours we think are important in email design, such has: multiple property declarations, selectors order, shorthands expansion, @media inclusion strategy, value parsing, pseudoselector support...

In email design it's usual to use some property redundance in order to achieve complete browser/client support. For example, take this code

```css
  .myhumbleclass {
  background-color: #5f9c8c; /* Fallback */
  background-color: rgba(95, 156, 140, 0.7);
  }
```

So, if your browser/client understands rgba colors, it will use them, otherways it will use the standard hexadecimal rgb color.
However most inliners will break this trick: in case o multiple property values, they will take only the last value, leaving an invalid rule for clients and browser that does not understand rgba colors.

In our test only Litmus inliner will take every single rule, even if duplicated, and put it in every piece of Dom indicated by selectors. 
In this case the resulting code will be huge and extremely verbose, but effective.

Styliner uses a kind of smart behaviour: it will maintain the duplicated properties only if in the same selector.
The assumption is that tricks like these are made within the single selector, not in multiple selectors. 
The resulting code is somehow less verbose, and in most case the tricks will work.

Media queries and MEDIA="xyz" style tag property are not replicable in inlined css, so, ideally, css inliners have to ignore them and leave them in the original format. 
In out tests turns out that this assumption is no always valid: only Litmus inliner follow this rule, while the other, in best case scenarios keeps correctly @media, while inlining MEDIA="SCREEN" style rules and properties (not so bad, but we think that every style with media indication has to be left untouched, potentially inlining of media screen could break for example the MEDIA="PRINT" rules), but in worst case we have inliners such as Premailer an Zurb that inlines almost every @media and MEDIA content, breaking the whole result.

Moreover the inliner has to decide what to do with the selectors and rules it does not understand or process: some decide to include the whole original style, like Campaign Monitor, Mailchimp and MailerMailer, some other tries to do some optimization, including only @media and unprocessed selectors (like Litmus), someother break everything trying to do this (Styliner), and, well, Torchbox strips everything (sigh).

Also the way of handling shorthand could bring some problem: in css properties order and shorthand use are important, and  shorthand splitting or aggregation and casual property reorder in Zurb, Premailer, Torchbox and Styliner could break your template in many subtle ways.




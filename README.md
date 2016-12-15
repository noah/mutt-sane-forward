# About

**Note: this is beta software.  It may shoot your dog.**

By default, [mutt](http://www.mutt.org/doc/manual/) does not come with
sane email forwarding.  As of the end of 2016, most people I know in my
personal and professional lives expect forwarded email mail to contain:

1. inline text representing the any textual emails in the thread up to that point; and
2. optionally, attachments.

Although mutt has two forwarding modes, neither one of them conforms
with the above expectation.  Specifically, mutt offers:

1. Option A) forward text AND attachments as a single MIME attachment.  This
   is particularly obnoxious and error-prone.
2. Option B) forward text and NO attachment.  This is totally useless.

3. Option C) bounce message.  Problem:  "bouncing" a message (not to be
   confused with a mailserver bounce message) is email voodoo/black
   magic, as you prefer.  And although "bouncing" a message will send a
   copy to the recipient, it will not be clear to that person that the
   message was forwarded.  Instead it will look to them as if they were
   on the original list of recipients, which is incorrect.  Further,
   bouncing will not leave a copy in the sender's Sent folder "Fwd: ..."
   demonstrating that she indeed forwarded the message.  TLDR: Stupid
   AND wrong.

So getting back to the preferred method, of sending text inline along
with (non-MIME) attachments ... it turns out that it is possible
within mutt.  The problem is that it requires one to remember and
repeat several unnecessary steps:

1. Select a given message
2. Switch to Attachments view (usual keybind: "v")
3. Tag all attachments by repeatedly pressing 't'
4. Press ;f
5. Answer "no" when prompted to "forward as attachment" (aka forward
   the whole thing as MIME)
6. Answer "yes" when prompted to "Can't decode all tagged attachments.
   MIME-forward the others? ([yes]/no)"

`$EDITOR` will then be opened, and the text shown inline, with PDFs and
so forth attached.

Problem with this?  It's got 5 steps too many and cannot be automated
with a macro, at least because `<tag-pattern>` does not work in mutt's
Attachment mode.  If you do not believe me then try typing `:push
<tag-pattern>.<enter>` in mutt's attachment view.

The upshot is that things like this simply do not work:

  `macro pager,index u '<view-attachments><tag-pattern>.<enter>'`

So as far as I can tell, a rather bloody hack is required, but it is a
bloody hack that happens to work extremely well.

# Dependencies

* zsh
* [pcre](https://www.archlinux.org/packages/core/i686/pcre/files/) is
  needed, or you can rewrite my multiline regexes in sed :(
* [munpack](https://aur.archlinux.org/packages/mpack/) is needed as the
  code is currently written, but conceiveably another attachment
  extraction tool, e.g.  [ripmime](http://www.pldaniels.com/ripmime/),
  could be rather trivially substituted.

# Installation

Add the following to your `.muttrc`:

        macro pager,index f '<pipe-message>/path/to/mutt-sane-forward<enter>' 'sanely forward an email'


# License

GPLv3; patches welcome.

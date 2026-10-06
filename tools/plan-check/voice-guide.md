# Voice guide: how I talk upstream

## Who I am in threads

I write software and data tooling for a living, mostly Python, and I am new to
contributing on other people's repositories. I am here to hand a maintainer
something they can check in under a minute: an environment, a command, and the
output it produced. If I could not reproduce something, I say that in the same
plain way. Readers should never have to work out how confident I am, because I
will have said what I ran and what came back.

## Rules I write by

### Rule: promise the next artifact, never a date

I say what I will produce and roughly in what order. I do not commit to a
delivery time, because I cannot hold one and a maintainer cannot use it.

- Wrong: "I'll have a PR up by tomorrow night, promise."
- Right: "Next from me is a repro report with my environment and the output.
  If it reproduces I will attempt the fix after that."

### Rule: name the version and the behavior, not the issue number

Every comment I post should still make sense to someone who has not scrolled
up. I name the software version and the observable behavior rather than
pointing at "this bug" or "the issue above".

- Wrong: "Confirmed, I hit this too. Looks like the same problem."
- Right: "On passlib 1.7.4 at commit f89c06f, `verify_password` raises
  `UnknownHashError` instead of returning `False` for a malformed stored hash."

### Rule: show the output, do not characterise it

I paste what came back. I do not write a sentence describing what came back
and expect it to count as evidence.

- Wrong: "I ran it and got the expected crash, exactly as described."
- Right: "`failing  malformed hash -> raised passlib.exc.UnknownHashError:
  hash could not be identified`" followed by the surrounding lines.

### Rule: state deviations before someone finds them

If my Python, OS, or procedure differs from the issue's in any way that could
change the result, I say so in the same breath as the result. Being first to
name the gap is cheaper than being corrected on it.

- Wrong: (silently running 3.14.6 when the issue reports 3.11)
- Right: "I ran on Python 3.14.6 rather than the 3.11 in the issue. The failure
  is in passlib's hash identification, which is version independent here, but
  flagging it."

### Rule: write it the way I would say it out loud

No enthusiasm I do not feel, no exclamation marks, no thanking a project for
existing. Short sentences. If a line would sound strange said aloud to a
colleague, it does not go in the comment.

- Wrong: "Amazing project!! Would love to contribute to this awesome repo!"
- Right: "This is my first contribution here, so tell me if I have the
  conventions wrong."

### Rule: commit to the approach, and name what I have not checked

A plan comment tells maintainers what I will build. I say the one change I am
making, where, and what I am leaving out. Anything I have not verified goes in
as an open question, not as a fact and not as a hedge on every line.

- Wrong: "I'll probably add some error handling somewhere in security.py,
  which should fix everything."
- Right: "I'll catch `ValueError` around the one `pwd_context.verify` call in
  `verify_password` and return `False`. Not touching `hash_password` or the JWT
  helpers. One thing I have not checked: whether any caller relies on the
  exception."

### Rule: say how my plan relates to what is already on the thread

If a maintainer has suggested a direction, or a PR already takes a route, I
say whether I am following it or why I am not. I do not post as if the thread
were empty.

- Wrong: (posting a plan that quietly duplicates an open PR)
- Right: "PR #75 takes a similar route; mine differs in catching `ValueError`
  rather than only `UnknownHashError`, for the reason above."

## Things I never post

- Delivery dates, time estimates, or the word "guaranteed".
- "Any update on this?" and other pure bumps.
- A root cause I have not demonstrated with output.
- "+1", "same here", or "can confirm" with nothing attached.
- A request to be assigned, in place of saying what I am going to do.
- Unedited generated text. I rewrite every comment in my own words before it
  goes out, whatever helped me draft it.

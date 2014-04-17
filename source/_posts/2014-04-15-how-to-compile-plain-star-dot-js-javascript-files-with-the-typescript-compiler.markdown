---
layout: post
title: "How to compile plain *.js (JavaScript) files with the TypeScript Compiler"
date: 2014-04-15 21:27:29 -0700
comments: true
categories: 
published: false
---

# Challenge

You've been tasked with spiking an initial port of your existing JavaScript solutions to [TypeScript](http://typescriptlang.com.

Your first hurdle can slow you down and be a bit challenging. The `tsc` (TypeScript compiler) requires all of your files end with a `.ts` file extension. This can make a quickly prototyping a port challenging. To get an idea of what a port to TypeScript will look like you don't want to deal with first renaming all of your files to .ts. Especially since there are probably files you want to not rename to TypeScript (like jQuery or AngularJS etc.

And since:

>TypeScript is a typed superset of JavaScript that compiles to plain JavaScript.

it seems reasonable that you could potentially get the benefits of the TypeScript compiler for even your existing JavaScript codebase.


# Challenge Accepted.

The TypeScript compiler is open source, so let's take a dive into the compiler to see if this is something we can work around...

### What are the steps we need to accomplish to make the compiler accept plain `.js` files

1. Get It
2. Copy it
3. Hack it
4. Use it

## Get it

Before we can get too far, let's first install the compiler onto our system.

I'll use [npm](http://npmjs.org) to install [typescript](https://www.npmjs.org/package/typescript)

    npm install -g typescript

Note the `-g` here tells `npm` to install typescript globally. This adds the TypeScript compiler to your `PATH` so you can get right at `tsc`'ing your TypeScript code.

## Copy it

We probably don't want to modify the globally installed version of `tsc` so we'll create a copy on our system to play around with. But, before we can do that we need to find where it is.

If on Windows or Mac

    where tsc

> NOTE: if you're using the PowerShell console on windows be sure to type out `where.exe tsc` because `where` is aliased to `Where-Object` in PowerShell which won't help us out in this case.

Once you've found the path to your version of `tsc`

Mine was in

-   `C:\Users\jason\AppData\Roaming\npm\tsc`
-   `C:\Users\jason\AppData\Roaming\npm\tsc.cmd`

Look at the contents of the `tsc.cmd` for Windows and `tsc` for non Windows machines. You'll notice that they are essentially executing `node.exe` passing in an argument to another `tsc` file in the `node_modules` path.

Take the two `tsc` and `tsc.cmd` files, copy them into a working folder `MyJSCompiler` and rename them. I named mine `jsc` [and he shall be my squishy](https://www.youtube.com/watch?v=iDOhFIX3sWE). Then take the contents of the `node_modules/typescript/*` folder (and path structure) and copy them to your working directory.

When you're done you should have a directory that looks something like this

```

﻿﻿--MyJSCompiler
  |   jsc                 <-- notice the re-named file from tsc -> jsc
  |   jsc.cmd             <-- notice the re-named file from tsc.cmd -> jsc.cmd
  |   
  ----node_modules
      ----.bin
      |       tsc
      |       tsc.cmd
      |       
      ----typescript
          |   .npmignore
          |   CopyrightNotice.txt
          |   LICENSE.txt
          |   package.json
          |   README.txt
          |   ThirdPartyNoticeText.txt
          |   
          ----bin
              |   lib.d.ts
              |   tsc
              |   tsc.js
              |   typescript.js
              |   
              ----resources
                  |   diagnosticMessages.generated.json
                  |   
                  ----(*.json files excluded for brevity)

```

Now you should be able to call your local version of `jsc` at the command line.

## Hack it

Since we have a local version we can hack on now, let's find out how to hack it. Thanks to [Ryan]((http://stackoverflow.com/users/1704166/ryan-cavanaugh) for already giving us a clue [here](http://stackoverflow.com/questions/17533301/can-i-compile-a-js-file-with-the-typescript-compiler-without-renaming-it-to-a/17533590#17533590).

So go open up the `node_modules/typescript/bin/tsc.js` file and apply the below diff/changes.

```diff
     function isTSFile(fname) {
-        return isFileOfExtension(fname, ".ts");
+        return isFileOfExtension(fname, ".ts") || isFileOfExtension(fname, ".js");
     }
```

## Use it

We've now implemented a small tweak to the TypeScript compiler that allows us to compile plain JavaScript files.

To use it, be sure to use the `--out FILE` or `--outDir DIRECTORY` options because if you don't the compiler will take the input javascript file and overwrite it with it's compiled version.

#### !!WARNING!! I'll say that again, to use it, be sure to use the `--out FILE` or `--outDir DIRECTORY` options because if you don't the compiler will take the input javascript file and overwrite the original with it's compiled version.

With source control, this can potentially be a fun experiment to see what TypeScript's version looks compared to your own, but I'll leave that up to you to play with.

Best of luck on your port to TypeScript.

---
layout: post
title: "Claude Code vs Cursor, July '25"
date: 2025-07-26
author: Andrei Radulescu-Banu
image: /assets/images/claude_code_vs_cursor.png
categories: [ai, programming, tech, reviews]
description: "Side-by-side comparison of Claude Code vs Cursor AI coding assistants in July 2025: speed, context window, code quality, and real-world workflow differences."
---

Today, I ran Anthropic [Claude Code](https://claude.ai/code) side by side with [Cursor](https://cursor.sh). Here is my take:

👉 Cursor operates within its own VSCode-like editor, while Claude Code functions as a command line tool.  
👉 With Cursor, I find it convenient to paste UI snapshots, especially when coding in React.  
👉 Claude Code allows for pasting screenshot images into the shell on Mac, but this feature is not available on Linux terminals.  
👉 The agentic flow in Claude Code outperforms that of Cursor. The latter often gets stuck in a loop.  
👉 The diff application in Cursor is superior to the text interface in Claude Code. I can easily amend diffs in Cursor. In Claude Code, that is not possible.  
👉 Claude Code excels in locating relevant files within the repository, including those in node modules that are not checked in.  
👉 However, Claude Code searches the sandbox each time, whereas Cursor builds an index for efficiency. Cursor is thus faster, and also pretty accurate.  
👉 I rarely rely on the Cursor file index. Instead of letting Cursor find the files it needs, I always pass them myself. It's a lot more accurate.  
👉 Cursor simplifies manual file attachment to the context compared to Claude Code.  
👉 While Cursor automatically attaches modified files in subsequent steps, Claude Code requires manual re-entry or dynamic file detection.  
👉 Both tools require active user involvement and problem-solving rather than just running as agents in the background.

In a recent coding task involving FormIO integration into DocRouter, Claude Code successfully resolved two challenging issues that Cursor couldn't tackle, and vice versa for another problem.

Our platform, [DocRouter.AI](https://docrouter.ai), utilizes Tailwind, while FormIO relies on Bootstrap, posing compatibility challenges.

Transitioning FormIO to Tailwind is complex, presenting difficulties for both Claude Code and Cursor.

## More about Claude Code

👉 At the AWS Summit in NYC I attended, Anthropic demonstrated Claude Code, but did not dwell on Claude Code agents – presumably, b/c they are not quite Enterprise grade.  
👉 But Anthropic uses Claude code agents internally, for sure.  
👉 They use Claude code to develop Claude code.  
👉 Claude code is implemented in Typescript directly, and installs as an npm package.

## Conclusion

👉 Each tool is better in different areas. At this point, I use both together – Cursor when I can, and Claude Code when Cursor can't solve it.  
👉 Claude Code could enhance its functionality with editor integration. It has a VSCode add-on, but that is still in its early stages.
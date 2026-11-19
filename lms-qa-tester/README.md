# LMS QA Tester

A Claude Code plugin that goes through an online course in a live LMS one slide at a time, in the browser. It then writes four separate reports:

1. **Technical bugs**: broken links, missing media, errors, layout, navigation and progress tracking.
2. **Grammar & spelling**
3. **Punctuation**
4. **Age suitability**: whether the content fits the target age group.

## Use

In any Claude Code session, run `/lms-qa-tester`, or say "QA this course". If you leave out the course URL or the age group, Claude asks for them. You sign in to the LMS yourself in the browser. Reports go to `lms-qa-reports/<course>-<date>/` in the folder the session is working in.

The plugin is read-only by default. Claude won't submit quizzes or forms, enrol, or pay unless you approve each action in chat.

## Install on another computer

Copy the whole `lms-qa-tester-marketplace` folder to the other computer and keep it somewhere it will stay. Then run these two commands, using the folder's path:

```bash
claude plugin marketplace add /path/to/lms-qa-tester-marketplace
```

```bash
claude plugin install lms-qa-tester@lms-qa-tools
```

Start a new session afterwards. If you put the folder on GitHub, you can use `owner/repo` in place of the path.

You need either the Claude desktop app (its built-in browser) or Claude Code with the Claude in Chrome extension. Node.js is not needed.

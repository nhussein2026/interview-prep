# Contributing to Interview Prep Hub

First off, **thank you** for considering contributing! 🎉

This project exists because of contributors like you. Whether you're fixing a typo or adding an entire new section, your help is appreciated.

---

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Adding a New Technology Stack](#adding-a-new-technology-stack)
- [Content Guidelines](#content-guidelines)
- [Pull Request Process](#pull-request-process)

---

## 📜 Code of Conduct

- Be respectful and inclusive
- Welcome newcomers warmly
- Focus on constructive feedback
- No spam or self-promotion

---

## 🤝 How Can I Contribute?

### 📝 Improve Existing Content

- Fix typos, grammar, or formatting issues
- Add more examples or explanations
- Update outdated information
- Add missing interview questions

### 📂 Add New Topics

Want to add a topic that doesn't exist? Great! Follow this structure:

```
topic-folder/
├── README.md          # Overview of the topic
├── concept-1.md       # Detailed notes on subtopic
├── concept-2.md
└── examples/          # Code examples (optional)
```

### 🆕 Request a New Section

Don't have time to write but have ideas? Open an issue with:
- The topic/stack you'd like to see
- Why it would be valuable
- Any resources you know of

---

## 🗂️ Adding a New Technology Stack

We'd love to expand coverage! Here's how to add a new stack:

### 1. Create the folder structure

```
XX-stack-name/              # XX = next number in sequence
├── README.md               # Stack overview
├── topic-1/
│   └── README.md
├── topic-2/
│   └── README.md
└── ...
```

### 2. Follow the naming convention

- Use lowercase with hyphens: `react-native`, `spring-boot`
- Number prefixes for ordering: `04-python-django`

### 3. Suggested stacks to add

| Stack | Folder Name |
|-------|-------------|
| Python/Django | `04-python-django` |
| Java/Spring Boot | `05-java-spring` |
| DevOps/Cloud | `06-devops-cloud` |
| Mobile (React Native/Flutter) | `07-mobile-dev` |
| Data Science/ML | `08-data-science` |
| Go/Golang | `09-golang` |
| .NET/C# | `10-dotnet` |

---

## ✍️ Content Guidelines

### Structure Each Topic

```markdown
# Topic Name

## Overview
2-3 sentences explaining what this is and why it matters.

## Key Concepts
- Concept 1: Brief explanation
- Concept 2: Brief explanation

## Common Interview Questions
1. Question one?
2. Question two?

## Code Examples
\`\`\`language
// Your code here
\`\`\`

## Tips for Interviews
- Practical tip 1
- Practical tip 2

## Resources
- [Resource Name](URL)
```

### Writing Tips

- ✅ Be **concise** - interviewers value clear, direct answers
- ✅ Use **code examples** where applicable
- ✅ Include **real interview questions** you've encountered
- ✅ Add **time/space complexity** for algorithms
- ✅ Link to **official documentation**
- ❌ Don't copy-paste from copyrighted sources
- ❌ Don't include irrelevant information

---

## 🔄 Pull Request Process

### 1. Fork & Clone

```bash
git clone https://github.com/YOUR_USERNAME/interview-prep.git
cd interview-prep
```

### 2. Create a Branch

```bash
git checkout -b feature/add-python-basics
# or
git checkout -b fix/typo-in-react-hooks
```

### 3. Make Your Changes

- Follow the content guidelines above
- Test any code examples you include
- Check for typos and formatting

### 4. Commit with Clear Messages

```bash
git add .
git commit -m "Add: Python data types and variables"
# or
git commit -m "Fix: Typo in React hooks explanation"
```

### 5. Push & Create PR

```bash
git push origin feature/add-python-basics
```

Then open a Pull Request on GitHub with:
- Clear title describing the change
- Brief description of what you added/changed
- Link to any related issues

---

## 🏷️ Commit Message Format

| Type | Description |
|------|-------------|
| `Add:` | New content or feature |
| `Fix:` | Bug fix or correction |
| `Update:` | Updating existing content |
| `Remove:` | Removing outdated content |
| `Docs:` | Documentation changes |

---

## ❓ Questions?

Open an issue with your question, and we'll be happy to help!

---

**Thank you for contributing! Together we can help developers worldwide ace their interviews. 🚀**

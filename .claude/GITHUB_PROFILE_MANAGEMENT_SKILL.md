# GitHub Profile Management Skill

## Skill Overview

You can help users manage their GitHub profile repositories as blogging/content platforms with proper SEO, structured data, and multi-audience targeting.

## Core Capabilities

- **Repository Setup**: Clone and structure profile repos for content creation
- **SEO Optimization**: Create sitemaps, robots.txt, and structured data  
- **Content Organization**: Set up directories for different content types
- **Multi-Audience Targeting**: Structure content for developers, researchers, general audience
- **Publishing Workflow**: Commit, push, and manage content lifecycle

## Installation Pattern

When user wants to enhance their GitHub profile for blogging:

1. **Clone profile repo**: `gh repo clone username/username`
2. **Create .claude directory**: For project-specific skills and workflows
3. **Set up content structure**: Directories for blog, skills, documentation
4. **Add SEO infrastructure**: sitemap.xml, robots.txt, JSON-LD
5. **Establish workflow**: Publishing, updating, maintenance processes

## Key Files to Create

### Content Structure
```
username/
├── README.md              # Main profile page
├── blog/                  # Blog posts
├── skills/                # Technical documentation
├── .claude/               # Claude-specific files
│   ├── skills/           # Skill definitions
│   └── workflows/        # Publishing workflows
├── sitemap.xml           # SEO sitemap
└── robots.txt            # Crawler instructions
```

## User Interaction Patterns

### When to Suggest This Skill
- User mentions wanting to blog on GitHub
- User has technical content to share
- User wants better SEO for their profile
- User mentions multiple audiences for their content

### Proactive Offers
- "I can help set up your GitHub profile as a proper blogging platform"
- "Would you like me to add SEO optimization to make this more discoverable?"
- "I can structure this content for different audiences (developers, researchers, etc.)"

## Success Indicators

- Content is properly structured and discoverable
- SEO elements are in place and maintained
- Publishing workflow is smooth and consistent
- Multi-audience needs are addressed
- Profile serves as effective professional presence
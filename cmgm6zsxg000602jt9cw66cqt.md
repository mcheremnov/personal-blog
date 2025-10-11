---
title: "How to Download Shopify Theme & Start Development"
datePublished: Sat Oct 11 2025 11:28:51 GMT+0000 (Coordinated Universal Time)
cuid: cmgm6zsxg000602jt9cw66cqt
slug: how-to-download-shopify-theme-and-start-development
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1760182079699/70219eb0-5827-4a81-86d0-36f725067afb.png
tags: cli, shopify, liquid, shopify-development

---

This guide was born from real frustration. I started with a simple question: "How do I download a Shopify theme and start developing?" What seemed like a straightforward task quickly became a journey through error messages and CLI confusion.

First, I hit the classic 404 GraphQL error when trying to connect to my dev store. Then came the cryptic "Couldn't find an app toml file" message because I was using the wrong commands. Each error led to research, troubleshooting, and eventually, solutions.

Rather than letting this knowledge scatter across browser tabs and terminal history, I decided to consolidate everything I learned into one comprehensive resource. This guide represents the path I wish I had when I started—covering not just the happy path, but the real issues developers encounter and how to solve them.

If you're reading this, you're probably facing similar challenges. I hope this guide saves you the hours of trial and error I went through.

---

## Introduction

This comprehensive guide will walk you through downloading a Shopify theme, setting up your development environment, and troubleshooting common issues.

## Prerequisites

Before you begin, ensure you have:

* Node.js (latest LTS version) installed
    
* A code editor (VS Code recommended)
    
* Staff access or owner permissions on your Shopify store
    
* An active Shopify store (not paused or frozen)
    

## Step 1: Install Shopify CLI

```bash
npm install -g @shopify/cli @shopify/theme
```

Verify installation:

```bash
shopify version
```

## Step 2: Authenticate With Your Store

```bash
shopify auth login
```

This opens a browser window where you'll log into your Shopify store and grant access to the CLI.

**Important:** If you encounter authentication issues later, you can re-authenticate:

```bash
shopify auth logout
shopify auth login
```

## Step 3: Download Your Shopify Theme

### Method 1: Using Shopify CLI (Recommended)

Navigate to your project directory and pull your theme:

```bash
# Navigate to your project folder
cd /Users/your-username/your-project-folder

# Pull theme from your store
shopify theme pull --store=your-store.myshopify.com
```

This command will:

* Show you a list of available themes on your store
    
* Let you select which theme to download
    
* Download all theme files to your current directory
    

### Method 2: Download from Shopify Admin

1. Log into your Shopify admin panel
    
2. Go to **Online Store &gt; Themes**
    
3. Find the theme you want to work on
    
4. Click the **Actions** button (three dots)
    
5. Select **Download theme file**
    
6. Extract the downloaded `.zip` file to your project directory
    

### Method 3: Start Fresh with Dawn Theme

If you're starting a new theme or want to use Shopify's reference theme:

```bash
# Initialize a new theme
shopify theme init

# Select "Dawn" when prompted
```

Or clone directly from GitHub:

```bash
git clone https://github.com/Shopify/dawn.git
cd dawn
```

## Step 4: Understanding Theme Structure

Your downloaded theme will have this structure:

```plaintext
your-theme/
├── assets/          # CSS, JS, images, fonts
├── config/          # Theme settings and configurations
├── layout/          # Base templates (theme.liquid)
├── locales/         # Translation files for internationalization
├── sections/        # Reusable, customizable sections
├── snippets/        # Reusable code blocks
└── templates/       # Page templates (product, collection, etc.)
```

**Key files to know:**

* `layout/theme.liquid` - The main wrapper for all pages
    
* `config/settings_schema.json` - Theme customization options
    
* `sections/*.liquid` - Dynamic, customizable content blocks
    
* `templates/*.json` - Page structure definitions
    

## Step 5: Start Development Server

```bash
shopify theme dev --store=your-store.myshopify.com
```

This command:

* Creates a local development server
    
* Provides a preview URL (usually [http://127.0.0.1:9292](http://127.0.0.1:9292))
    
* Enables hot reload (changes appear instantly)
    
* Creates a development theme on your store
    

**Access your development preview:**

* Local: Open the URL shown in your terminal
    
* Shopify: Go to Online Store &gt; Themes and find your development theme
    

## Common Issues & Troubleshooting

### Error: "Couldn't find an app toml file"

**Problem:** You're using app commands instead of theme commands.

**Solution:** Always use `shopify theme` commands for theme development:

```bash
# Correct commands for themes
shopify theme pull
shopify theme dev
shopify theme push

# NOT these (these are for app development)
shopify dev
shopify deploy
```

### Error: "404 Not Found" or GraphQL Connection Error

**Possible causes and solutions:**

1. **Incorrect store URL format**
    
    * Use: [`your-store.myshopify.com`](http://your-store.myshopify.com)
        
    * Don't use: [`https://your-store.myshopify.com`](https://your-store.myshopify.com) or custom domains
        
2. **Authentication expired**
    
    ```bash
    shopify auth logout
    shopify auth login
    ```
    
3. **Insufficient permissions**
    
    * Ensure you're the store owner or have staff access
        
    * Collaborators need "Themes" permission enabled
        
    * Check: Shopify Admin &gt; Settings &gt; Users and permissions
        
4. **Store not active**
    
    * Verify your store isn't paused or frozen
        
    * Check your store status in Shopify admin
        
5. **Outdated Shopify CLI**
    
    ```bash
    npm update -g @shopify/cli @shopify/theme
    ```
    
6. **Specify store explicitly**
    
    ```bash
    shopify theme dev --store=your-store.myshopify.com
    ```
    

### Alternative: Using Theme Kit (Legacy Tool)

If Shopify CLI continues to fail, you can use Theme Kit:

```bash
# Install Theme Kit
gem install theme_kit

# Download theme
theme get --password=[your-private-app-password] --store=your-store.myshopify.com
```

**To get a private app password:**

1. Shopify Admin &gt; **Apps**
    
2. **App and sales channel settings**
    
3. **Develop apps** &gt; **Create an app**
    
4. Give it theme read/write permissions
    
5. Generate and copy the access token
    

## Development Workflow Best Practices

### Push Changes to Your Store

```bash
# Push all changes
shopify theme push --store=your-store.myshopify.com

# Push to a specific theme
shopify theme push --theme=THEME_ID
```

### Work Safely

* **Never edit your live theme directly**
    
* Always work on a development or duplicate theme
    
* Test thoroughly before publishing
    
* Use version control (Git) for your theme files
    

### Set Up Version Control

```bash
git init
git add .
git commit -m "Initial theme setup"
```

Create a `.gitignore` file:

```plaintext
.DS_Store
node_modules/
config/settings_data.json
```

### Publish Your Theme

Once you're ready to go live:

1. Test everything in your development theme
    
2. In Shopify Admin, go to **Online Store &gt; Themes**
    
3. Find your development theme
    
4. Click **Actions &gt; Publish**
    

Or push and publish via CLI:

```bash
shopify theme push --theme=THEME_ID --publish
```

## Essential Theme Development Commands

```bash
# Pull theme from store
shopify theme pull --store=your-store.myshopify.com

# Start development server
shopify theme dev --store=your-store.myshopify.com

# Push changes to store
shopify theme push --store=your-store.myshopify.com

# List all themes on your store
shopify theme list --store=your-store.myshopify.com

# Create a new theme
shopify theme init

# Check for theme errors
shopify theme check

# Share preview of your development theme
shopify theme share
```

## Next Steps in Theme Development

Now that your environment is set up, you can:

* **Customize Liquid templates** - Learn Shopify's templating language
    
* **Modify sections** - Create dynamic, customizable content blocks
    
* **Add custom CSS/JS** - Enhance styling and functionality
    
* **Use theme settings** - Add customization options for merchants
    
* **Test responsiveness** - Ensure mobile compatibility
    
* **Optimize performance** - Improve loading speeds
    

## Resources

* [Shopify Theme Documentation](https://shopify.dev/docs/themes)
    
* [Liquid Documentation](https://shopify.dev/docs/api/liquid)
    
* [Dawn Theme GitHub](https://github.com/Shopify/dawn)
    
* [Shopify CLI Documentation](https://shopify.dev/docs/themes/tools/cli)
    

## Quick Reference: Common Scenarios

**Scenario: First time setup**

```bash
cd your-project-folder
shopify theme pull --store=your-store.myshopify.com
shopify theme dev --store=your-store.myshopify.com
```

**Scenario: Connection issues**

```bash
shopify auth logout
shopify auth login
shopify theme dev --store=your-store.myshopify.com
```

**Scenario: Starting fresh**

```bash
shopify theme init
cd your-new-theme
shopify theme dev --store=your-store.myshopify.com
```

---

You're now ready to start developing your Shopify theme! Remember to always work on a development theme and keep backups of your work.

Hopefully this guide could come in handy, and wash off some of the frustration that I had in the beginning. As always feel free to express your thoughts and criticize this piece! Cheers
---
layout: single
title: Migrating Jekyll to Astro Blog
tags:
  - jekyll
  - astro
  - blog
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/blog-with-astro.png
  overlay_image: /assets/2025/02/blog-with-astro.png
excerpt: Moving Jekyll to Astro Blog
---

This blog is using Jekyll with _Minimal Mistakes_ theme and I quite okay with this. Just to step out my curiousity, I tried to move this blog from Jekyll to Astro.

# Tool
I am using [Mike Farah's `yq`](https://github.com/mikefarah/yq) to change the blog.

# Creating Blog
For the sake of completeness.

```bash
pnpm create astro@latest jpmrblood.github.io.astro
cd jpmrblood.github.io.astro/
pnpm astro add tailwindcss -y
```

The prompt from Astro 5.2 gives me option to install dependencies, that's why I don't need to run `pnpm install`.

# Remove `/blog` slug

By default, Astro blog put its blog post in `/blog` slug. I'm removing it by:
- Moving `[...slug].astro` to `pages`
- Copy `./src/pages/blog/blog.astro` to `./src/pages/index.astro` with
- Adjust the imports.


# Treat The Files

## Moving

- Move `./_posts` to `./src/content/blog`
- Move `./assets` to `./public/assets`

## Create A Script

```bash
#!/bin/bash

# Define the target directory
directory="./src/content/blog"

# Check if the target is not a directory
if [ ! -d "$directory" ]; then
  exit 1
fi


# Loop over every file matching the pattern in the specified directory
for file in "$directory"/*; do
    # Extract the date part (YYYY-MM-DD) from the filename
    date=$(echo "$file" | grep -Eo '[[:digit:]]{4}-[[:digit:]]{2}-[[:digit:]]{2}' )
    
    # Extract the filename without the date (the part after the date)
    new_filename=$(echo "$file" | sed -E 's/.\/src\/content\/blog\/[0-9]{4}-[0-9]{2}-[0-9]{2}-(.*)/.\/src\/content\/blog\/\1/')
    
    # Rename the file to remove the date
    mv $file $new_filename

    # Add pubDate if not present
    if [ "$(yq -f="extract" '.pubDate' "$new_filename")" = "null" ]; then
        echo "no pubDate"
        yq -i -f="process" ".pubDate = \"$date\"" "$new_filename"
    fi

    # Remove tags and categories. Process hero image.
    if [ ! "$(yq -f="extract" '.header.overlay_image' "$new_filename")" = "null" ]; then
        yq -i -f="process" '.heroImage = .header.overlay_image' "$new_filename"
        yq -i -f="process" '.prefix = .categories[0]' "$new_filename" 
        yq -i -f="process" 'del .tags | del .header | del .categories' "$new_filename"
    fi

    # Excerpt
    if [ "$(yq -f="extract" '.excerpt' "$new_filename")" = "null" ]; then
        echo "Put Title to Description in $new_filename"
        yq -i -f="process" '.description = .title' "$new_filename"
    else
        echo "Change Excerpt to Description $new_filename"
        yq -i -f="process" '.description = .excerpt' "$new_filename"
        yq -i -f="process" 'del .excerpt' "$new_filename"
    fi
    

    echo "Processed $file -> $new_filename..."
    echo " "
done
```

Basically here's what we do with the script:
1. Rename the blog post files from `[DATE]-title.md` to `title.md`
2. Read the `[DATE]` in file name and put it into `pubDate`
3. Create `heroImage` from `header.overlay_image`. Remove `header` and its children after that.
4. Create `prefix` from the first value of `categories`. Remove `categories` after that.
5. Remove `tags`.
6. Create `description` with the following as value:
   a. If `excerpt` is exists, use the content. Delete `excerpt`.
   b. If `excerpt` not exists, use the `title` content. 

## Edit Blogpost Slug

Create `./src/utils/slug.ts`

```js
export function formatSlug(pubDate: string | Date, title: string): string {
	const date = new Date(pubDate);
	const year = date.getFullYear();
	const month = String(date.getMonth() + 1).padStart(2, '0');
	const day = String(date.getDate()).padStart(2, '0');

	return `${year}/${month}/${day}/${title.toLowerCase().replace(/\s+/g, '-')}`;
}
```

Modify `./src/pages/[...slug].astro` to update the slug static path.
```js
// ...
import { formatSlug } from '../utils/slug'; // Import the helper function

export async function getStaticPaths() {
	const posts = await getCollection('blog');
	return posts.map((post) => ({
		params: { slug: formatSlug(post.data.pubDate, post.id) },
		props: post,
	}));
}
type Props = CollectionEntry<'blog'>;
// ...
```

## Edit index.astro

Edit the `index.astro` which formerly from `blog.astro` with custom slug function.

```js
//...
// Helper function to format slug using pubDate and title
function formatSlug(pubDate: string | Date, title: string): string {
	const date = new Date(pubDate);
	const year = date.getFullYear();
	const month = String(date.getMonth() + 1).padStart(2, '0'); // Ensure two digits for month
	const day = String(date.getDate()).padStart(2, '0'); // Ensure two digits for day

	// Generate slug with date and title, replacing spaces in the title with hyphens
	return `${year}/${month}/${day}/${title.toLowerCase().replace(/\s+/g, '-')}`;
}

// Get the blog collection, sort it by pubDate, and generate slugs
const posts: (CollectionEntry<'blog'> & { slug: string })[] = (await getCollection('blog')).map((post) => ({
	...post,
	slug: formatSlug(post.data.pubDate, post.id), // Generate slug based on pubDate and title
})).sort(
	(a, b) => new Date(b.data.pubDate).valueOf() - new Date(a.data.pubDate).valueOf(),
);
// ...
```

## Edit content.config.ts

Modify the data:
```js
const blog = defineCollection({
	// Load Markdown and MDX files in the `src/content/blog/` directory.
	loader: glob({ base: './src/content/blog', pattern: '**/*.{md,mdx}' }),
	// Type-check frontmatter using a schema
	schema: z.object({
		title: z.string(),
		description: z.string(),
		// Transform string to Date object
		pubDate: z.coerce.date(),
		updatedDate: z.coerce.date().optional(),
		heroImage: z.string().optional(),
		prefix: z.string().optional(),
		created: z.date().optional(),
		modified: z.date().optional(),
	}),
});
```
I want to put prefix into the slug next time.

## And other minor changes

Yeah, some personalizations in header file and footer not worth writing.

# What Doesn't Work

I'm not really good with Javascript and TypeScript. This move only for me to re-learn JavaScript. I have passed the trauma when React changed its license and rewrote its approach when I already deep into making boilerplates in the past for my startup company. I'm getting back to this Javascript world again. 

The thing missing for me:
- Astro blog still missing the table functionality, especially when you have header in the column.
- I need to convert liquid image to Github markdown syntax for images.
- Astro blog cannot handle tags. I need to create some functionalities.
- Astro search not implemented. My knowledge is lacking there.

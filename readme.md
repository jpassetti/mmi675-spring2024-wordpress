### MMI 675 Advanced Web Design and Development - Spring 2024

# Outline: Wordpress theme development

An 8 week progression for a course on WordPress theme development. The course is designed for beginners with basic knowledge of HTML, CSS, and PHP. The goal is to provide a comprehensive understanding of WordPress theme development, including custom post types, advanced custom fields, and responsive design.

## Resources

- [Local WP](https://localwp.com/)
- [WordPress Codex](https://codex.wordpress.org/)
- [Advanced Custom Fields Pro](https://www.advancedcustomfields.com/pro/)
- [WP_Query](https://developer.wordpress.org/reference/classes/wp_query/)
- [WordPress Developer Handbook](https://developer.wordpress.org/)
- [WordPress Theme Developer Handbook](https://developer.wordpress.org/themes/)
- [WordPress Community](https://make.wordpress.org/)
- [WordPress.tv](https://wordpress.tv/)
- [WordPress Stack Exchange](https://wordpress.stackexchange.com/)
- [WordPress Subreddit](https://www.reddit.com/r/Wordpress/)

## Weeks 1-4: First Project (Small Blog)

### Week 1: Introduction and Basic Setup

#### Session 1: Introduction

- Introduction to WordPress, themes, and the importance of local development environments.
- Set up a local development environment with [Local WP](https://localwp.com/), install a WordPress site and login.
- Overview of WordPress dashboard, posts, pages, and initial theme setup.
- Start the small blog project by creating an empty theme directory `/wp-content/themes/newhouse-blog`.
- Create an `index.php` file and a `style.css` file with the necessary theme information. Explain that the `style.css` file, specifically the theme header, is required for WordPress to recognize the theme.
- Activate the theme and preview the changes.

Place simple text inside `index.php`.

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Newhouse Blog</h1>
</body>
</html>
```

Include the theme header in `style.css`.

```css
/*
Theme Name: Newhouse Blog
Author: Your Name
Description: A simple blog theme for learning WordPress theme development.
Version: 1.0
*/

body {
  background-color: yellow;
}
```

Link the theme's stylesheet to `index.php` with `get_stylesheet_uri()`.

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="<?php echo get_stylesheet_uri(); ?>">
</head>
<body>
    <h1>Newhouse Blog</h1>
</body>
</html>
```

Translation: "Get the URL of the current theme's stylesheet and output it here."

#### Session 2: Homepage

- Introduction to the [WordPress template hierarchy](https://developer.wordpress.org/themes/basics/template-hierarchy/) and the role of theme files.
- Create a basic HTML structure for the blog's header, footer, and main content area inside `index.php`.
- Style the blog with CSS, focusing on basic typography, spacing, colors, and layout.
- Hardcode the blog's homepage content and style it with CSS.

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="<?php echo get_stylesheet_uri(); ?>">
</head>
<body>
    <header>
    <h1>Newhouse Blog</h1>
    </header>
    <main>
        <h2>Latest Posts</h2>
        <article>
            <h3>Post Title</h3>
            <p>Post Excerpt</p>
        </article>
        <article>
            <h3>Post Title</h3>
            <p>Post Excerpt</p>
        </article>
        <article>
            <h3>Post Title</h3>
            <p>Post Excerpt</p>
        </article>
    </main>
    <footer>
        <p>Copyright 2024 Newhouse Blog</p>
    </footer>
</body>
</html>
```

```css
/* style.css */
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
}
header {
  background-color: #333;
  color: #fff;
  padding: 1rem;
  text-align: center;
}
main {
  padding: 1rem;
}
footer {
  background-color: #333;
  color: #fff;
  padding: 1rem;
  text-align: center;
}
```

### Week 2: Theme Structure and Customization

#### Session 1: Blog Posts

- Introduction to the [WordPress Loop](https://codex.wordpress.org/The_Loop) and the role of the `index.php` file.
- Create a basic loop to display blog posts on the homepage.
- Customize the blog post layout with CSS. Focus on responsive design and typography.

```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
    <article>
            <h3><?php the_title(); ?></h3>
            <h4><?php the_date(); ?></h4>
            <?php the_excerpt(); ?>
            <p><a href="<?php the_permalink(); ?>">Read more</a></p>
    </article>

<?php endwhile; else : ?>
	<p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>
```

```css
/* style.css */
h2 {
  margin-bottom: 2rem;
  font-size: 2rem;
  font-weight: bold;
}
article {
  margin-bottom: 2rem;
  padding: 1rem;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
article h3 {
  margin-bottom: 0.5rem;
  font-size: 1.5rem;
  font-weight: bold;
}
article h4 {
  margin-bottom: 1rem;
  font-size: 1.25rem;
  font-weight: bold;
}
article p {
  margin-top: 1rem;
  font-size: 1rem;
}
```

#### Session 2: Single Post and Page Templates

- Create `header.php` and `footer.php` files and include them in `index.php`.
- Introduction to single post and page templates. Create `single.php` and `page.php` files.
- Insert the WordPress Loop into `single.php` and `page.php` to display single post and page content.
- Explain how the Wordpress Loop is multi-purpose and can be used in different contexts. When used on the homepage, it displays multiple posts. When used on a single post or page, it displays the content of that post or page.
- Create a post and a page in the WordPress dashboard and preview the changes.

```php
<?php get_header(); ?>
<main>
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
            <h2><?php the_title(); ?></h2>
            <?php the_content(); ?>
<?php endwhile; else : ?>
    <p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>
</main>
<?php get_footer(); ?>
```

### Week 3: WordPress Loop and Theme Development

#### Session 1: Template tags and featured images

- Review WordPress template tags, like `the_title()`, `the_date()`, `the_excerpt()`, and `the_permalink()`.
- Edit the `header.php` with the blog's title and description using `bloginfo()`.
- Read more about [bloginfo()](https://developer.wordpress.org/reference/functions/bloginfo/).
- Why would using `bloginfo()` be better than hardcoding the blog's title and description?

```php
<header>
    <h1><?php bloginfo( 'name' ); ?></h1>
    <p><?php bloginfo( 'description' ); ?></p>
</header>
```

- Introduction to featured images (also referred to as post thumnail). Create a `functions.php` file and add support for featured images.
- Read more about [the_post_thumbnail()](https://developer.wordpress.org/reference/functions/the_post_thumbnail/)

```php
<?php
// inside functions.php
add_theme_support( 'post-thumbnails' );
```

- Output the featured image in the blog post loop and single post template.
- Explain why it's important to check if a post has a featured image before displaying it.

```php
<?php if ( has_post_thumbnail() ) {
    the_post_thumbnail(); ?>
} ?>
```

- Customize the featured image size and alignment with CSS.

```php
<?php the_post_thumbnail(
    'medium', // size options: thumbnail, medium, large, full
    array(
        'class' => 'showcase' // custom class
    )
); ?>
```

```css
/* style.css */
.showcase {
  width: 100%;
  height: auto;
}
```

#### Session 2: Customizing the Blog

- Introduction to WordPress menus. Create a custom menu in the WordPress dashboard and display it in the theme.
- Register a menu location in `functions.php`.
- Add items to the menu from the WordPress dashboard > Menus.
- Display the menu in the theme's header with `wp_nav_menu()`.

```php
<?php
// inside functions.php
register_nav_menus( array(
    'primary' => esc_html__( 'Primary Menu', 'newhouse-blog' ),
) );
```

```php
<?php
// inside header.php
wp_nav_menu( array(
    'theme_location' => 'primary', // specify the menu location, as registered in functions.php
    'container' => false, // remove the default <div> container
    'menu_class' => 'menu' // add a custom class
) );
?>
```

### Week 4: Sidebar and Finalizing the Small Blog Project

#### Session 1: Creats a sidebar to display latest posts

- Create `sidebar.php` and include it in `single.php`.
- Include it with `get_sidebar()`.
- Create a loop to display the latest posts in the sidebar.

```php
<?php get_header(); ?>
    <main>
    <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
            <h2><?php the_title(); ?></h2>
            <?php the_content(); ?>
    <?php endwhile; else : ?>
        <p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
    <?php endif; ?>
    </main>
    <?php get_sidebar(); ?>
<?php get_footer(); ?>
```

```php
// inside sidebar.php
<?php if ( have_posts() ) : ?>
    <aside>
        <h2>Latest Posts</h2>
        <ul>
            <?php while ( have_posts() ) : the_post(); ?>
                <li><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></li>
            <?php endwhile; ?>
        </ul>
    </aside>
<?php endif; ?>
```

- Style the sidebar with CSS and make it responsive.
- Explain the different between `main` and `sidebar`, specifically how they are used with SEO in mind.

#### Session 2: Finalizing the Small Blog Project

- Review the small blog project and discuss the importance of responsive design.
- Test the blog's responsiveness and make necessary adjustments.
- Review the project's code and discuss best practices.
- Discuss the importance of ongoing learning and resources for WordPress theme development.

## Weeks 5-8: Second Project (Netflix Site)

### Week 5: Planning and Custom Post Types

#### Session 1: Planning the faux Netflix site.

- Discuss the project's requirements and how custom post types and taxonomies can be used to organize content.
- Custom post types: films and tv shows.
- Custom taxonomies: genres.
- Install Advanced Custom Fields Pro (ACF Pro) and discuss its role in the project.

#### Session 2: Creating custom post types and taxonomies.

- Utilize ACF Pro to create custom post types and taxonomies.
- Create field groups and fields for customizing the content of films (hold off on tv shows).
- Add items to the `genres` taxonomy like action, comedy, drama, horror, and sci-fi.
- Create a [WP_Query](https://developer.wordpress.org/reference/classes/wp_query/) to display films and genres on the faux Netflix site.
- WP_Query is a powerful tool for customizing the WordPress Loop and displaying specific content.

```php
<?php
// inside index.php
$films = new WP_Query( array(
    'post_type' => 'film',
    'posts_per_page' => -1
) );

if ( $films->have_posts() ) : while ( $films->have_posts() ) : $films->the_post(); ?>
    <article>
        <h2><?php the_title(); ?></h2>
        <?php the_content(); ?>
    </article>
<?php endwhile; endif; ?>
```

Translation: "Get all films, check if there are any, and loop through them."

### Week 6: Taxonomy Loop and Advanced Custom Fields

#### Session 1: Loop through the `genres` taxonomy and output films.

- Replace the code on `index.php` with a loop that displays films by genre.
- Create a custom query to display films by genre.
- "A loop within a loop": loop through genres and display films for each genre.

```php
<?php
// inside index.php
$genres = get_terms( array(
    'taxonomy' => 'genre',
    'hide_empty' => false
) );

foreach ( $genres as $genre ) { ?>
   <section>
    <h2><?php print $genre->name; ?></h2>

    <?php
    $films = new WP_Query( array(
        'post_type' => 'film',
        'posts_per_page' => -1,
        'tax_query' => array(
            array(
                'taxonomy' => 'genre',
                'field' => 'slug',
                'terms' => $genre->slug
            )
        )
    ) );

    if ( $films->have_posts() ) : while ( $films->have_posts() ) : $films->the_post(); ?>
        <article>
            <h3><?php the_title(); ?></h3>
            <?php the_content(); ?>
        </article>
    <?php endwhile; endif;
} // end of foreach loop
?>
```

#### Session 2: Advanced ACF Pro features.

- Introduction to ACF Pro's custom fields, like fields for duration (number), release date (date), and rating (checkboxes).
- Output the custom fields in the film loop and style them with CSS.

```php
// inside the loop on single.php
if ( $films->have_posts() ) : while ( $films->have_posts() ) : $films->the_post(); ?>
        <article>
            <h3><?php the_title(); ?></h3>
            <div class="meta-information">
                <p>Duration: <?php the_field( 'duration' ); ?> minutes</p>
                <p>Release Date: <?php the_field( 'release_date' ); ?></p>
                <p>Rating: <?php the_field( 'rating' ); ?></p>
            </div>
            <?php the_content(); ?>
        </article>
    <?php endwhile; endif; ?>
```

- Discuss what other information could be useful for the faux Netflix site, like actors, directors, and trailers, and possible ways to implement them.

### Week 7: TV Shows

#### Session 1: TV Shows and Custom Fields

- Create a custom post type for TV shows and add a repeater field for seasons and episodes.
- Each season will have sub-fields: title (text), release date (date), and episodes (repeater).
- Each episode will have sub-fields: title (text), release date (date), description (textarea), cover image (image), and video (oEmbed).

#### Session 2: Displaying TV Shows and Finalizing the Faux Netflix Site

- Create a WP_Query loop on `index.php` to display TV shows.
- Utilize `the_permalink()` to link to the single TV show page.
- Create a custom single template for TV shows: `single-tv_show.php`.
- Create a loop to display the single TV show and its seasons.
- Output the episodes for each season and style them with CSS.
- Paste YouTube URLs into the video oEmbed field. When outputting the video, WordPress will automatically embed the video player for you.

```php
<?php
// Inside single-tv_show.php
if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
    <main>
        <h1><?php the_title(); ?></h1>
        <?php the_content(); ?>

        <?php
        $seasons = get_field('seasons');
        if ($seasons) {
            foreach ($seasons as $season) { ?>
                <section class="season">
                    <h2><?php print $season['title']; ?></h2>
                    <p>Release Date: <?php print $season['release_date']; ?></p>

                    <?php
                    $episodes = $season['episodes'];
                    if ($episodes) {
                        foreach ($episodes as $episode) { ?>
                            <article class="episode">
                                <h3><?php print $episode['title']; ?></h3>
                                <p>Release Date: <?php print $episode['release_date']; ?></p>
                                <p><?php print $episode['description']; ?></p>
                                <?php if ($episode['cover_image']) { >
                                    <img src="<?php print $episode['cover_image']['url']; ?>" alt="<?php print $episode['cover_image']['alt']; ?>">
                                <?php } ?>
                                <?php print $episode['video'];  ?>
                            </article>
                        <?php }
                    } ?>
                </section>
            <?php }
        } ?>
    </main>
<?php endwhile; endif; ?>
```

### Week 8: Finalizing and Review

#### Session 1:

- Finalizing the faux Netflix site.
- Implementing final touches like search.php.
- Testing the site's responsiveness and making necessary adjustments.

#### Session 2

- Course wrap-up.
- Discussing deployment, WordPress community resources, and ongoing learning opportunities.
- Review of best practices and Q&A session.

### Open-Ended Final Project Assignment: Enhancing the Netflix Site

#### Objective:

Your task is to take the foundational Netflix site we've built and enhance it by adding new features, functionalities, and content types. Think about what makes streaming platforms engaging and how you can replicate these aspects in your project.

#### Guidelines:

- **Research and Inspiration:** Start by exploring real-world streaming platforms. Note the features that stand out to you, especially those that contribute to user engagement and content discovery. Consider how these platforms showcase their content, including trailers, actor profiles, and photo galleries.
- **Planning:** Before you start coding, outline the features you want to add. Consider how these will integrate with your existing site structure. Plan your custom post types, taxonomies, fields, and any new page templates you might need.

#### Implementation:

You have the freedom to choose the features you want to add, but here are some ideas to get you started:

- **People (Actors, Directors, etc.):** Create a custom post type for people involved in TV shows and films. Include custom fields for biographies, filmography, and possibly a connection to the TV shows or films they've worked on.
- **Trailers:** Integrate trailers for your TV shows or films. This could involve adding a new field to your existing custom post types or creating a separate post type for trailers that can be linked to your content.
- **Photo Gallery:** Implement a photo gallery feature where users can view high-quality images from TV shows and films. Consider using a WordPress plugin like Lightbox for a more interactive experience.
- **Enhanced Search and Filtering:** Improve how users can search and filter TV shows and films. This might include filtering by genre, release year, or even by cast members.
- **Testing and Evaluation:** After implementing your features, thoroughly test your site. Consider usability, performance, and how well your enhancements integrate with the existing design and functionality.

#### Reflection and Documentation:

Write a brief report detailing your enhancements, the choices you made, and any challenges you encountered. Include screenshots and explanations of how your features work.

#### Deliverables:

- A fully functional enhanced Netflix site incorporating the new features.
- A report that includes a summary of added features, design choices, technical challenges, and how you overcame them.

#### Evaluation Criteria:

- Creativity and innovation in feature selection and implementation.
- Technical execution and code quality.
- Usability and integration of new features with the existing site design and functionality.
- Quality and clarity of the project report.

#### Submission Deadline: Friday, May 10, 2024

### Conclusion

Throughout the course, encourage students to experiment and explore WordPress features and functionalities beyond the scope of the lesson plans. This hands-on approach not only solidifies their understanding but also inspires creativity and problem-solving skills.

By using Wordpress as a CMS, whether a publishing platform or a full-fledged web application, students will gain valuable experience in web development and content management. This course will provide a strong foundation for students to continue learning and growing in the field of web design and development.

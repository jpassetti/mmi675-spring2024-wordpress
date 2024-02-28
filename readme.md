### MMI 675 Advanced Web Design and Development - Spring 2024

# Outline: Wordpress theme development

An 8 week progression for a course on WordPress theme development. The course is designed for beginners with basic knowledge of HTML, CSS, and PHP. The goal is to provide a comprehensive understanding of WordPress theme development, including custom post types, advanced custom fields, and responsive design.

## Weeks 1-4: First Project (Small Blog)

### Week 1: Introduction and Basic Setup

#### Session 1: Introduction

- Introduction to WordPress, themes, and the importance of local development environments.
- Set up a local development environment with Local WP, install a WordPress site and login.
- Overview of WordPress dashboard, posts, pages, and initial theme setup.
- Start the small blog project by creating an empty theme directory `/wp-content/themes/newhouse-blog`.
- Create an index.php file and a style.css file with the necessary theme information.
- Activate the theme and preview the changes.

#### Session 2: Homepage

- Introduction to the WordPress template hierarchy and the role of theme files.
- Create a basic HTML structure for the blog's header, footer, and main content area for `index.php`.
- Style the blog with CSS, focusing on basic typography, spacing, colors, and layout.
- Hardcode the blog's homepage content and style it with CSS.

### Week 2: Theme Structure and Customization

#### Session 1: Blog Posts

- Introduction to the [WordPress Loop](https://codex.wordpress.org/The_Loop) and the role of the `index.php` file.
- Create a basic loop to display blog posts on the homepage.
- Customize the blog post layout with CSS and introduce the concept of responsive design.

```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
    <article>
            <h2><?php the_title(); ?></h2>
            <h3><?php the_date(); ?></h3>
            <?php the_excerpt(); ?>
            <p><a href="<?php the_permalink(); ?>">Read more</a></p>
    </article>

<?php endwhile; else : ?>
	<p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>
```

#### Session 2: Single Post and Page Templates

- Create header.php and footer.php files and include them in `index.php`.
- Introduction to single post and page templates. Create `single.php` and `page.php` files.
- Insert the WordPress Loop into `single.php` and `page.php` to display single post and page content.
- Explain how the Wordpress Loop is multi-purpose and can be used in different contexts.

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
- Edit the header.php with the blog's title and description using `bloginfo()`.

```php
<header>
    <h1><?php bloginfo( 'name' ); ?></h1>
    <p><?php bloginfo( 'description' ); ?></p>
</header>
```

- Introduction to featured images. Create a `functions.php` file and add support for featured images.

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
- Add items to the menu from the WordPress dashboard.
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
    'container' => false, // remove the <div> container
    'menu_class' => 'menu' // add a custom class
) );
?>
```

### Week 4: Sidebar and Finalizing the Small Blog Project

#### Session 1: Creats a sidebar to display latest posts

- Create sidebar.php and include it in `index.php` and `single.php`.
- Create a loop to display the latest posts in the sidebar.

```php
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

- **Session 1:** Finalizing the faux Netflix site. Implementing final touches like search.php. Testing the site's responsiveness and making necessary adjustments.
- **Session 2:** Course wrap-up. Discussing deployment, WordPress community resources, and ongoing learning opportunities. Review of best practices and Q&A session.

### Open-Ended Final Project Assignment: Enhancing the Netflix Site

#### Objective:

Your task is to take the foundational Netflix site you've built and enhance it by adding new features, functionalities, and content types. Think about what makes streaming platforms engaging and how you can replicate these aspects in your project.

#### Guidelines:

- **Research and Inspiration:** Start by exploring real-world streaming platforms. Note the features that stand out to you, especially those that contribute to user engagement and content discovery. Consider how these platforms showcase their content, including trailers, actor profiles, and photo galleries.
- **Planning:** Before you start coding, outline the features you want to add. Consider how these will integrate with your existing site structure. Plan your custom post types, taxonomies, fields, and any new page templates you might need.

#### Implementation:

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

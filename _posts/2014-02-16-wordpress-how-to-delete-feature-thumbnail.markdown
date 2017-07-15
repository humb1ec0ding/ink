---
layout: post
title: "[wordpress] feature thumbnail 없애기"
date: 2014-02-16 17:08:25 +0900
comments: true
categories: etc
tags: blog
---

Wordpress 사용중에 **featured image** 를 이용하고 있었는데 최근에 다음과 같은 문제가 발생하고 있다.

* 글 포스팅에 없는 엉뚱한 thumbnail 이 featured image 로 선택되는 경우.
* Featured image 가 기존에 생성된 경우에 이를 없애기 어려움.

Theme 에서 **featured image**를 잘 조절이 안 되는 경우에는 원하지 않는 이미지가 중복으로 표시가 되니 참 마음에 안 드는 경우가 있었습니다. 그런데 좀 찾아보니 바로 해답이 있더군요.

<!--more-->

#### [Removing featured image thumbnail from post ](http://wordpress.org/support/topic/removing-featured-image-thumbnail-from-post)

	Hi, I think you'd need to manually remove this from the theme's page.php or post.php file depending on whether it's a post or page.

	You can access the relevant file via the WordPress admin area >> Appearance >> Editor >> You'll then see an option to select the file on the right. This is the code that makes up the page/post and there should be area that has "the_post_thumbnail();" in it. If you remove that section it will remove the featured thumbnail. If you're not sure what to remove paste a copy of the code here and I'll be able to advise.

소스 상에서 **featured image** 표시하기 위한 코드인 **the_post_thumbnail()** 을 찾아서 잘 지워주면 된다고 하네요.

```php
	<div id="post-<?php the_ID(); ?>" <?php post_class(); ?>>
		<?php if ( is_sticky() && is_home() && ! is_paged() ) : ?>
		<div class="featured-post">
			<?php _e( 'Featured post', 'readr' ); ?>
		</div>
		<?php endif; ?>
		<div class="entry-header">
			<?php the_post_thumbnail(); ?>   <=== !!!
			<?php if ( is_single() ) : ?>
			<h1 class="entry-title"><?php the_title(); ?></h1>
			<?php else : ?>
			<h1 class="entry-title">
				<a href="<?php the_permalink(); ?>" title="<?php echo esc_attr( sprintf( __( 'Permalink to %s', 'readr' ), the_title_attribute( 'echo=0' ) ) ); ?>" rel="bookmark"><?php the_title(); ?></a>
			</h1>
			<?php endif; // is_single() ?>
			<?php readr_entry_date(); ?>
		</div><!-- .entry-header -->
```

해보니깐 역시나 잘 됩니다. :)        

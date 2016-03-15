---
title: WordPress自动添加文章版权信息
layout: post
---
我们通常都会选择在 WordPress 文章末尾处添加版权信息，当别人转载时就要带上本文链接。我们希望博文发表时，这些信息被自动添加，而无需手动输入，这就需要更改一些代码。网上的教程都有些过时了，不适用于较新版本的Wordpress，本文就向大家介绍如何自动在最新版Wordpress的文章末尾处添加版权声明。

先在主题目录下添加一个 copyright.php 文件，内容如下：

```php
<div style="width:100%; margin: 5px 10px 5px 0px;">

<p style="text-align: left;">

除非注明，<a href="<?php echo get_settings('home'); ?>"><strong>一维博客</strong></a>文章均为原创，转载请以链接形式标明本文地址。

<br />

本文地址：<a href="<?php urldecode(get_permalink()) ?>"><?php echo urldecode(get_permalink()) ?></a>

<br />

</p>

</div>
```

可以看到，我们调用了 urldecode() 函数，这样如果你的 permanent link 中包含的中文就不会乱码了。注意：该文件是新添加的，我们要确保它是UTF-8编码的。

然后再当前使用的主题目录下找到 content.php ,进行编辑，在如下位置插入代码：

```php
<div class="entry-content">
		<?php
			/* translators: %s: Name of current post */
			the_content( sprintf(
				__( 'Continue reading %s', 'twentyfifteen' ),
				the_title( '<span class="screen-reader-text">', '</span>', false )
			) );

			include(TEMPLATEPATH."/copyright.php");//新插入的代码

			wp_link_pages( array(
				'before'      => '<div class="page-links"><span class="page-links-title">' . __( 'Pages:', 'twentyfifteen' ) . '</span>',
				'after'       => '</div>',
				'link_before' => '<span>',
				'link_after'  => '</span>',
				'pagelink'    => '<span class="screen-reader-text">' . __( 'Page', 'twentyfifteen' ) . ' </span>%',
				'separator'   => '<span class="screen-reader-text">, </span>',
			) );
		?>
	</div><!-- .entry-content -->
```

这样，在每次发博文时，文章结尾就能自动添加版权信息了！
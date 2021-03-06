---
title: Actions
---

This page lists all actions in Meta Box plugin.

## `rwmb_before`

This action runs before any meta box is shown. It takes 1 parameter: the instance of `RW_Meta_Box` class for current meta box.

```php
do_action( 'rwmb_before', $this );
```

## `rwmb_before_{$meta_box_id}`

This action is the same as `rwmb_before` but is applied for a specific meta box (identified by ID). It accepts same parameters.

## `rwmb_after`

This action runs after any meta box is shown. It takes 1 parameter: the instance of `RW_Meta_Box` class for current meta box.

```php
do_action( 'rwmb_after', $this );
```

## `rwmb_after_{$meta_box_id}`

This action is the same as `rwmb_after` but is applied to a specific meta box (identified by ID). It accepts same parameters.

## `rwmb_before_save_post`

This action runs before any meta box is saved into post meta. It takes 1 parameter: current post ID.

```php
do_action( 'rwmb_before_save_post', $post_id );
```

## `rwmb_{$meta_box_id}_before_save_post`

This action is the same as `rwmb_before_save_post` but is applied to a specific meta box (identified by ID). It accepts same parameters.

## `rwmb_after_save_post`

This action runs after any meta box is saved into post meta. It takes 1 parameter: current post ID.

```php
do_action( 'rwmb_after_save_post', $post_id );
```

## `rwmb_{$meta_box_id}_ after_save_post`

This action is the same as `rwmb_after_save_post` but is applied to a specific meta box (identified by ID). It accepts same parameters.

## `rwmb_enqueue_scripts`

This action runs after all Meta Box scripts and styles are enqueued to allows developers to enqueue more scripts and styles. It takes 1 parameter: the instance of `RW_Meta_Box` class for current meta box.

```php
do_action( 'rwmb_enqueue_scripts', $this );
```
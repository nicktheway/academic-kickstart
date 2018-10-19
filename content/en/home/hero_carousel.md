+++
# Hero Carousel widget.
widget = "hero_carousel"
active = true
date = 2018-10-20T00:00:00

# Order that this section will appear in.
weight = 1

# Slide interval.
# Use `false` to disable animation or enter a time in ms, e.g. `5000` (5s).
interval = 5000

# Minimum slide height.
# Specify a height to ensure a consistent height for each slide.
height = "380px"

# Slides.
# Duplicate an `[[item]]` block to add more slides.
[[item]]
  title = "The hidden music box"
  content = "<br>Video game project :smile:<br><br>"
  align = "center"  # Choose `center`, `left`, or `right`.

  # Overlay a color or image (optional).
  #   Deactivate an option by commenting out the line, prefixing it with `#`.
  #overlay_color = "#676"  # An HTML color value.
  overlay_img = "headers/THMB_demo_1.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.2  # Darken the image. Value in range 0-1.

  # Call to action button (optional).
  #   Activate the button by specifying a URL and button label below.
  #   Deactivate by commenting out parameters, prefixing lines with `#`.
  cta_label = "Demo 2: Trailer"
  cta_url = "https://www.youtube.com/watch?v=ISdjUexxoyg"
  cta_icon_pack = "fa"
  cta_icon = "gamepad"

[[item]]
  title = "Latest University Project"
  content = "Pagerank Algorithm, Parallel implementation with C and openMP<br>"
  align = "left"

  overlay_color = "#555"  # An HTML color value.
  overlay_img = "headers/pagerank_diades.png"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.8  # Darken the image. Value in range 0-1.

  cta_label = "Check it on github"
  cta_url = "https://github.com/nicktheway/Pagerank"
  cta_icon_pack = "fa"
  cta_icon = "angle-right"

[[item]]
  title = "Meet me on Codingame"
  content = "A place where you can have coding challenges and competitions!<br>"
  align = "right"

  overlay_color = "#333"  # An HTML color value.
  overlay_img = "headers/NTW.jpeg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.2  # Darken the image. Value in range 0-1.

  cta_label = "My profile"
  cta_url = "https://www.codingame.com/profile/8706762a498d0146c1762b65a4eebde42853401"
  cta_icon_pack = "fa"
  cta_icon = "angle-right"
+++

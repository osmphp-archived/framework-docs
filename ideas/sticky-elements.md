# Sticky Elements

Sticky means that when an element is scrolled out, it is put into a `position: fixed` container. When user scrolls back, the the element is moved back to its original position. A view can stick to the top, to the left or to the right of the browser window. 

Expected sticky behavior in standard backend theme:  

* `#title` sticks to the top
* `#sidebar` sticks to the left
* `#grid` header sticks to the top

Mark sticky elements with `Sticky` view model:

    <div id="title"></div>
    <script>new Osm_Ui_StickyElements.StickToTop('#title', {sort_order: 10})</script>
    
    <div id="sidebar"></div>
    <script>new Osm_Ui_StickyElements.StickToSide('#sidebar', {side: 'left'})</script>    
    
    <div id="grid__header"></div>
    <script>new Osm_Ui_StickyElements.StickToTop('#grid__header', {sort_order: 20})</script>

2 important mobile-related behaviors:

1. When scrolling down top stickies gradually slide out of the view, when scrolling up they gradually slide back in. Like mobile browser's title bar.
2. On mobile sidebar is hidden by default and may show up by clicking a button as a drawer. on desktop sidebar is shown by default and may hide by clicking the same button. As in `osmdocs.com`. In addition to that, on desktop it sticks to the side. "Button" should be available on every page where sidebar is shown, most often as a "Sidebar" checkbox in the `#title.menu`.    
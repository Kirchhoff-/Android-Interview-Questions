With the advent of Android Lollipop, the **RecyclerView** made its way officially. The **RecyclerView** is much more powerful, flexible and a major enhancement over **ListView**.

- ViewHolder Pattern
In a **ListView**, it was recommended to use the **ViewHolder** pattern but it was never a compulsion. In case of **RecyclerView**, this is mandatory using the **RecyclerView.ViewHolder** class.

-  LayoutManager
This is another massive enhancement brought to the **RecyclerView**. In a **ListView**, the only type of view available is the vertical **ListView**. There is no official way to even implement a horizontal **ListView**.
Now using a **RecyclerView**, we can have a
    - LinearLayoutManager - which supports both vertical and horizontal lists,
    - StaggeredLayoutManager - which supports Pinterest like staggered lists,
    - GridLayoutManager - which supports displaying grids as seen in Gallery apps.

-  Item Animator
**ListViews** are lacking in support of good animations, but the **RecyclerView** brings a whole new dimension to it. Using the **RecyclerView.ItemAnimator** class, animating the views becomes so much easy and intuitive.

-  Item Decoration
In case of **ListViews**, dynamically decorating items like adding borders or dividers was never easy. But in case of **RecyclerView**, the **RecyclerView.ItemDecorator** class gives huge control to the developers but makes things a bit more time consuming and complex.

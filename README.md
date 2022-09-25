# UseItemdecorationOffsets

<img width="300" alt="スクリーンショット 2022-09-25 13 55 54" src="https://user-images.githubusercontent.com/47273077/192128991-9e2d4f82-ce3d-4a18-b226-feff6dcc7cbe.png">
DividerItemDecoration
```kt
class DividerItemDecoration(color: Int, private val heightInPixels: Int) : RecyclerView.ItemDecoration() {

    private val paint = Paint()

    init {
        paint.color = color
        paint.isAntiAlias = true
    }

    override fun onDraw(c: Canvas, parent: RecyclerView, state: RecyclerView.State) {
        super.onDraw(c, parent, state)
        val left = parent.paddingLeft
        val right = parent.width - parent.paddingRight
        val childCount = parent.childCount
        for (i in 0 until childCount) {
            val child = parent.getChildAt(i)
            val params = child.layoutParams as RecyclerView.LayoutParams
            val top = child.bottom + params.bottomMargin
            val bottom = top + heightInPixels
            c.drawRect(left.toFloat(), top.toFloat(), right.toFloat(), bottom.toFloat(), paint)
        }
    }
}

```

FavoritesFragment
```kt
  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    favoritesRecyclerView.layoutManager = LinearLayoutManager(activity)
    favoritesRecyclerView.adapter = adapter
    val heightInPixels = resources.getDimensionPixelSize(R.dimen.list_item_divider_height)
    context?.let {
      favoritesRecyclerView.addItemDecoration(
        DividerItemDecoration(
          ContextCompat.getColor(it, R.color.black), heightInPixels))
    }
  }
  ```

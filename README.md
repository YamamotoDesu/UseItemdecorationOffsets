# UseItemdecorationOffsets

## Draw Item Separators
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

-------

## Challenge: Draw a Grid
<img width="300" alt="スクリーンショット 2022-09-25 14 49 12" src="https://user-images.githubusercontent.com/47273077/192130277-80166a62-f528-4c26-93c8-036f22c45048.png">


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
  
  CreatureActivity
  ```kt
    private fun setupFoods(){
    foodRecyclerView.layoutManager = GridLayoutManager(this, 3, LinearLayoutManager.VERTICAL, false)
    foodRecyclerView.adapter = adapter
    val foods = CreatureStore.getCreatureFoods(creature)
    adapter.updateFoods(foods)
    val dividerWidthInPixels = resources.getDimensionPixelSize(R.dimen.list_item_divider_height)
    foodRecyclerView.addItemDecoration(
      FoodItemDecoration(
        ContextCompat.getColor(this, R.color.black), dividerWidthInPixels
      ))
  }
  ```
  
  ```kt
  class FoodItemDecoration(color: Int, private val dividerWidthInPixels: Int) : RecyclerView.ItemDecoration() {

    private val paint = Paint()

    init {
        paint.color = color
        paint.isAntiAlias = true
    }

    override fun onDraw(c: Canvas, parent: RecyclerView, state: RecyclerView.State) {
        super.onDraw(c, parent, state)

        val childCount = parent.childCount
        for (i in 0 until childCount) {
            val child = parent.getChildAt(i)
            val params = child.layoutParams as RecyclerView.LayoutParams

            var left = parent.paddingLeft
            var right = parent.width - parent.paddingRight
            var top = child.top + params.topMargin
            var bottom = top + dividerWidthInPixels
            c.drawRect(left.toFloat(), top.toFloat(), right.toFloat(), bottom.toFloat(), paint)

            left = child.right - params.rightMargin
            right = left + dividerWidthInPixels
            top = parent.paddingTop
            bottom = parent.height - parent.paddingBottom

            c.drawRect(left.toFloat(), top.toFloat(), right.toFloat(), bottom.toFloat(), paint)
        }
    }
}
```

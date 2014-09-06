title: TDD Demo GameOfLife with Android Studio
date: 2014-09-06 19:27:05
tags:
---
On my Java class the teacher used [Conway's Game of Life](http://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) as an example for [TDD](http://en.wikipedia.org/wiki/Test-driven_development) and I found it a really good example. So I made some videos to demonstrate how I tried TDD on it.

For each step I also posted new or changed code, and links of full code of that step on [GitHub](https://github.com/eidiot/TDD-GameLife-AndroidStudio).

I posted some keyboard shortcuts as well. You can find and change them in "Keymap" section of Android Studio's preference panel.

## 1. Create GameOfLife Project

Simply create a new project on [Android Studio](http://developer.android.com/sdk/installing/studio.html). [Android Studio](http://developer.android.com/sdk/installing/studio.html) is still in beta (I'm using 0.8.6) but I am very happy with it so far. On the [GDG Auckland September Meetup](http://www.meetup.com/GDGAuckland/events/199648182/) Julius Spencer said they've already been using it on production for one year now. So give it a try if you haven't yet. It's so much better than eclipse!

- Run...: option + control + R
- Run last: control + R

<!-- more -->

## 2. Create class `GameModel` and test case `GameModelTest`

- Create class: (New...) command + N
- Create test: (Show Intention Actions) option + Enter

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/02TestCase/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public class GameModelTest extends TestCase {
    public void setUp() throws Exception {
        super.setUp();
    }
}

```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/02TestCase/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public class GameModel {
}
```

## 3. `GameModel` initialisation

- New test method: (New...) command + N
- Create non-existent method or class: (Show Intention Actions) option + Enter
- Duplicate Line or Block: command + D

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/03Init/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public class GameModelTest extends TestCase {
    GameModel instance;
    public void setUp() throws Exception {
        super.setUp();
        instance = new GameModel(3, 3);
    }
    public void test_init() throws Exception {
        assertEquals(3, instance.getRows());
        assertEquals(3, instance.getColumns());
    }
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/03Init/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public class GameModel {
    private int rows;
    private int columns;
    public GameModel(int rows, int columns) {
        this.rows = rows;
        this.columns = columns;
    }
    public int getRows() {
        return rows;
    }
    public int getColumns() {
        return columns;
    }
}
```

## 4. `GameModel.isAlive()`

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/04IsAlive/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_is_alive() throws Exception {
    assertFalse(instance.isAlive(0, 0));
}

```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/04IsAlive/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public boolean isAlive(int row, int column) {
    return false;
}
```

## 5. `GameModel.makeAlive()`

- Go to test (Or go to test subject in test case): command + shift + T

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/05MakeAlive/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_make_alive() throws Exception {
    instance.makeAlive(0, 0);
    assertTrue(instance.isAlive(0, 0));
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/05MakeAlive/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
private boolean map[][];
public GameModel(int rows, int columns) {
    map = new boolean[rows][columns];
}
public boolean isAlive(int row, int column) {
    return map[row][column];
}
public void makeAlive(int row, int column) {
    map[row][column] = true;
}
```

## 6. Test out of map

- Refactor/Extract/Method...: command + option + M

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/06OutOfMap/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_is_alive() throws Exception {
    assertFalse(instance.isAlive(0, 0));
    assertFalse(instance.isAlive(-1, 0));
    assertFalse(instance.isAlive(0, -1));
    assertFalse(instance.isAlive(3, 0));
    assertFalse(instance.isAlive(0, 3));
}
public void test_make_alive() throws Exception {
    instance.makeAlive(0, 0);
    assertTrue(instance.isAlive(0, 0));
    instance.makeAlive(-1, 0);
    instance.makeAlive(0, -1);
    instance.makeAlive(3, 0);
    instance.makeAlive(0, 3);
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/06OutOfMap/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public boolean isAlive(int row, int column) {
    return !isOutOfMap(row, column) && map[row][column];
}
public void makeAlive(int row, int column) {
    map[row][column] = true;
    if (!isOutOfMap(row, column)) {
        map[row][column] = true;
    }
}
private boolean isOutOfMap(int row, int column) {
    return row < 0 || row > rows - 1 || column < 0 || column > columns - 1;
}
```

## 7. `GameModel.makeDead()`

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/07MakeDead/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_make_dead() throws Exception {
    instance.makeAlive(0, 0);
    instance.makeDead(0, 0);
    assertFalse(instance.isAlive(0, 0));
    instance.makeDead(-1, 0);
    instance.makeDead(0, -1);
    instance.makeDead(3, 0);
    instance.makeDead(0, 3);
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/07MakeDead/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public void makeDead(int row, int column) {
    if (!isOutOfMap(row, column)) {
        map[row][column] = false;
    }
}
```

## 8. Rule 1 to Rule 3

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/08Rule1To3/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_live_cell() throws Exception {
    instance.makeAlive(1, 1);
    assertFalse(instance.willLive(1, 1));
    instance.makeAlive(0, 0);
    assertFalse(instance.willLive(1, 1));
    instance.makeAlive(0, 1);
    assertTrue(instance.willLive(1, 1));
    instance.makeAlive(0, 2);
    assertTrue(instance.willLive(1, 1));
    instance.makeAlive(2, 2);
    assertFalse(instance.willLive(1, 1));
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/08Rule1To3/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public boolean willLive(int row, int column) {
    int aliveNeighbours = 0;
    for (int i = row - 1; i <= row + 1; i++) {
        for (int j = column - 1; j <= column + 1; j++) {
            if (!(i == row && j == column) && isAlive(i, j)) {
                aliveNeighbours++;
            }
        }
    }
    if (isAlive(row, column)) {
        return aliveNeighbours == 2 || aliveNeighbours == 3;
    }
    return false;
}
```

## 9. Rule 4

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/09Rule4/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_dead_cell() throws Exception {
    assertFalse(instance.willLive(1, 1));
    instance.makeAlive(0, 0);
    assertFalse(instance.willLive(1, 1));
    instance.makeAlive(0, 1);
    assertFalse(instance.willLive(1, 1));
    instance.makeAlive(0, 2);
    assertTrue(instance.willLive(1, 1));
    instance.makeAlive(2, 2);
    assertFalse(instance.willLive(1, 1));
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/09Rule4/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public boolean willLive(int row, int column) {
    int aliveNeighbours = 0;
    for (int i = row - 1; i <= row + 1; i++) {
        for (int j = column - 1; j <= column + 1; j++) {
            if (!(i == row && j == column) && isAlive(i, j)) {
                aliveNeighbours++;
            }
        }
    }
    if (isAlive(row, column)) {
        return aliveNeighbours == 2 || aliveNeighbours == 3;
    }
    return aliveNeighbours == 3;
}
```

## 10. `GameModel.next()`

```java GameModelTest.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/10Next/app/src/androidTest/java/me/eidiot/gameoflife/GameModelTest.java View Full Code
public void test_live_cell() throws Exception {
    //...
    assertFalse(instance.willLive(1, 1));
    instance.next();
    assertFalse(instance.isAlive(1, 1));
}
public void test_dead_cell() throws Exception {
    //...
    assertFalse(instance.willLive(1, 1));
    instance.next();
    assertFalse(instance.isAlive(1, 1));
}
```

```java GameModel.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/10Next/app/src/main/java/me/eidiot/gameoflife/GameModel.java View Full Code
public void next() {
    boolean newMap[][] = new boolean[rows][columns];
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < columns; j++) {
            newMap[i][j] = willLive(i, j);
        }
    }
    map = newMap;
}
```

## 11. GameView

- Recent Files: command + E
- Override Methods...: control + O

```java GameView.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/11GameView/app/src/main/java/me/eidiot/gameoflife/GameView.java View Full Code
public class GameView extends View {
    private GameModel model;
    private Paint strokePaint;
    private Paint fillPaint;
    public GameView(Context context) {
        super(context);
    }
    public GameView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public GameView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    public void setup(GameModel model) {
        this.model = model;
        strokePaint = new Paint();
        strokePaint.setStyle(Paint.Style.STROKE);
        strokePaint.setColor(Color.LTGRAY);
        fillPaint = new Paint();
        fillPaint.setStyle(Paint.Style.FILL);
        fillPaint.setColor(Color.BLACK);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        int size = 20;
        for (int i = 0; i < model.getRows(); i++) {
            for (int j = 0; j < model.getColumns(); j++) {
                int left = size * i;
                int top = size * j;
                int right = left + size;
                int bottom = top + size;
                if (model.isAlive(i, j)) {
                    canvas.drawRect(left, top, right, bottom, fillPaint);
                }
                canvas.drawRect(left, top, right, bottom, strokePaint);
            }
        }
    }
}
```

```xml activity_main.xml https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/11GameView/app/src/main/res/layout/activity_main.xml View Full Code
<RelativeLayout>
    <me.eidiot.gameoflife.GameView
        android:id="@+id/gameView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</RelativeLayout>
```

```java MainActivity.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/11GameView/app/src/main/java/me/eidiot/gameoflife/MainActivity.java View Full Code
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        GameModel model = new GameModel(30, 15);
        GameView view = (GameView) findViewById(R.id.gameView);
        view.setup(model);
    }
}
```

## 12. Run the game and Glider demo

```java MainActivity.java https://github.com/eidiot/TDD-GameLife-AndroidStudio/blob/12Glider/app/src/main/java/me/eidiot/gameoflife/MainActivity.java View Full Code
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final GameModel model = new GameModel(30, 15);
        model.makeAlive(1, 0);
        model.makeAlive(2, 1);
        model.makeAlive(0, 2);
        model.makeAlive(1, 2);
        model.makeAlive(2, 2);
        final GameView view = (GameView) findViewById(R.id.gameView);
        view.setup(model);
        final int INTERVAL = 300;
        final Handler handler = new Handler();
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                handler.postDelayed(this, INTERVAL);
                model.next();
                view.invalidate();
            }
        };
        handler.postDelayed(runnable, INTERVAL);
    }
}
```

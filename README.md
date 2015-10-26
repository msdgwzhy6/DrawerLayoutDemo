# DrawerLayoutDemo
有关Material Design新特性的详解。
源码地址：https://github.com/chrisbanes/cheesesquare
自己添加备注后的地址：https://github.com/heinika/DrawerLayoutDemo
英文教程：https://guides.codepath.com/android/Handling-Scrolls-with-CoordinatorLayout
中文教程：http://my.oschina.net/kooeasy/blog/484593
主要包含：
DrawerLayout
NavigationView
CoordinatorLayout
AppBarLayout
Toolbar
TabLayout
ViewPager
FloatingActionButton
CollapsingToolbarLayout
NestedScrollView
CardView
RecyclerView

具体使用：
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    android:id="@+id/drawer_layout"
    tools:context=".MainActivity">
    <!--DrawerLayout里的第一个为 content -->
    <include layout="@layout/include_list_viewpager" />

    <!--DrawerLayout里的第二个为drawer
     headerLayout为标题的布局
     menu为list的配置文件
     颜色都是从主题中提取的，可用如下属性改变
     app:itemIconTint=""
     app:itemBackground=""
     app:itemTextColor=""
     -->
    <android.support.design.widget.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/nav_header"
        app:menu="@menu/drawer_view"
        >

    </android.support.design.widget.NavigationView>
</android.support.v4.widget.DrawerLayout>
public class MainActivity extends AppCompatActivity {

    private DrawerLayout mDrawerLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //设置actionbar
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        //设置actionbar的图片并设置为可点击
        final ActionBar ab = getSupportActionBar();
        ab.setHomeAsUpIndicator(R.drawable.ic_menu);
        ab.setDisplayHomeAsUpEnabled(true);

        mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);

        //drawer菜单
        NavigationView navigationView = (NavigationView) findViewById(R.id.nav_view);
        if (navigationView != null) {
            //设置菜单可选择，并且选择后关闭
            setupDrawerContent(navigationView);
        }

        //设置ViewPager
        ViewPager viewPager = (ViewPager) findViewById(R.id.viewpager);
        if (viewPager != null) {
            setupViewPager(viewPager);
        }

        //设置tablayout，viewpager上的标题
        TabLayout tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setupWithViewPager(viewPager);
        tabLayout.setSelectedTabIndicatorColor(getResources().getColor(R.color.colorPrimaryDark));  //设置tab下方颜色

        //设置点击对勾后，触发的提示
        FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Here's a Snackbar", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });
    }

    //设置点击图标拉出抽屉
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case android.R.id.home:
                mDrawerLayout.openDrawer(GravityCompat.START);   //设置打开的方向
                return true;
        }
        return super.onOptionsItemSelected(item);
    }

    //actionbar省略号的设置
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.sample_actions, menu);
        return true;
    }

    /**
     * 设置菜单可选择，并且选择后关闭
     * @param navigationView drawer菜单
     */
    private void setupDrawerContent(NavigationView navigationView) {
        navigationView.setNavigationItemSelectedListener(
                new NavigationView.OnNavigationItemSelectedListener() {
                    @Override
                    public boolean onNavigationItemSelected(MenuItem menuItem) {
                        menuItem.setChecked(true);
                        mDrawerLayout.closeDrawers();
                        return true;
                    }
                });
    }

    /**
     * 设置item
     * @param viewPager
     */
    private void setupViewPager(ViewPager viewPager) {
        Adapter adapter = new Adapter(getSupportFragmentManager());
        adapter.addFragment(new CheeseListFragment(), "Category 1");
        adapter.addFragment(new CheeseListFragment(), "Category 2");
        adapter.addFragment(new CheeseListFragment(), "Category 3");
        viewPager.setAdapter(adapter);
    }

    //自定义adapter
    static class Adapter extends FragmentPagerAdapter {
        private final List<Fragment> mFragments = new ArrayList<>();
        private final List<String> mFragmentTitles = new ArrayList<>();

        public Adapter(FragmentManager fm) {
            super(fm);
        }

        public void addFragment(Fragment fragment, String title) {
            mFragments.add(fragment);
            mFragmentTitles.add(title);

        }

        @Override
        public Fragment getItem(int position) {
            return mFragments.get(position);
        }

        @Override
        public int getCount() {
            return mFragments.size();
        }

        @Override
        public CharSequence getPageTitle(int position) {
            return mFragmentTitles.get(position);
        }
    }
}
<?xml version="1.0" encoding="utf-8"?>
<!--
  ~ Copyright (C) 2015 The Android Open Source Project
  ~
  ~ Licensed under the Apache License, Version 2.0 (the "License");
  ~ you may not use this file except in compliance with the License.
  ~ You may obtain a copy of the License at
  ~
  ~      http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing, software
  ~ distributed under the License is distributed on an "AS IS" BASIS,
  ~ WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  ~ See the License for the specific language governing permissions and
  ~ limitations under the License.
-->

<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main_content"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/detail_backdrop_height"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        android:fitsSystemWindows="true">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleMarginStart="48dp"
            app:expandedTitleMarginEnd="64dp">
            <!-- 设置图片-->
            <!--添加一个定义了app:layout_collapseMode="parallax" 属性的ImageView，出现和消失会有过度-->
            <ImageView
                android:id="@+id/backdrop"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                android:fitsSystemWindows="true"
                app:layout_collapseMode="parallax" />
            <!-- 设置标题-->
            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
                app:layout_collapseMode="pin" />

        </android.support.design.widget.CollapsingToolbarLayout>

    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:paddingTop="24dp">

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_margin="@dimen/card_margin">

                <LinearLayout
                    style="@style/Widget.CardContent"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content">

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="Info"
                        android:textAppearance="@style/TextAppearance.AppCompat.Title" />

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/cheese_ipsum" />

                </LinearLayout>

            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="@dimen/card_margin"
                android:layout_marginLeft="@dimen/card_margin"
                android:layout_marginRight="@dimen/card_margin">

                <LinearLayout
                    style="@style/Widget.CardContent"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content">

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="Friends"
                        android:textAppearance="@style/TextAppearance.AppCompat.Title" />

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/cheese_ipsum" />

                </LinearLayout>

            </android.support.v7.widget.CardView>

            <android.support.v7.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="@dimen/card_margin"
                android:layout_marginLeft="@dimen/card_margin"
                android:layout_marginRight="@dimen/card_margin">

                <LinearLayout
                    style="@style/Widget.CardContent"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content">

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="Related"
                        android:textAppearance="@style/TextAppearance.AppCompat.Title" />

                    <TextView
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/cheese_ipsum" />

                </LinearLayout>

            </android.support.v7.widget.CardView>

        </LinearLayout>

    </android.support.v4.widget.NestedScrollView>

    <!--app:layout_anchor="@id/appbar" 固定到appbar上 -->
    <android.support.design.widget.FloatingActionButton
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        app:layout_anchor="@id/appbar"
        app:layout_anchorGravity="bottom|right|end"
        android:src="@drawable/ic_discuss"
        android:layout_margin="@dimen/fab_margin"
        android:clickable="true"/>

</android.support.design.widget.CoordinatorLayout>

public class CheeseDetailActivity extends AppCompatActivity {

    public static final String EXTRA_NAME = "cheese_name";

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detail);

        Intent intent = getIntent();
        final String cheeseName = intent.getStringExtra(EXTRA_NAME);

        final Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

        CollapsingToolbarLayout collapsingToolbar =
                (CollapsingToolbarLayout) findViewById(R.id.collapsing_toolbar);
        /**
         * 设置CollapsingToolbarLayout的标题文字
         */
        collapsingToolbar.setTitle(cheeseName);
        //设置CollapsingToolbarLayout的背景图
        loadBackdrop();
    }

    /**
     * 设置CollapsingToolbarLayout的图片
     */
    private void loadBackdrop() {
        final ImageView imageView = (ImageView) findViewById(R.id.backdrop);
        Glide.with(this).load(Cheeses.getRandomCheeseDrawable()).centerCrop().into(imageView);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.sample_actions, menu);
        return true;
    }
}

[android之ViewPager修改滑动速度 - 那年盛夏 - 博客园 (cnblogs.com)](https://www.cnblogs.com/cmai/p/7705190.html)

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Fragment.HomeFragment">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/home_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/white">

        <androidx.constraintlayout.widget.ConstraintLayout
            android:id="@+id/have_layout"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/header_imageview">
            <androidx.constraintlayout.widget.Guideline
                android:id="@+id/guideline3"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                app:layout_constraintGuide_percent="0.9" />
            <androidx.constraintlayout.widget.Guideline
                android:id="@+id/guideline4"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                app:layout_constraintGuide_percent="0.1" />

            <androidx.cardview.widget.CardView
                android:id="@+id/constraintLayout"
                android:layout_width="0dp"
                android:layout_height="170dp"
                android:layout_marginTop="30dp"
                android:background="@color/white"
                app:cardCornerRadius="15dp"
                app:layout_constraintEnd_toStartOf="@+id/cardView2"
                app:layout_constraintHorizontal_chainStyle="packed"
                app:layout_constraintHorizontal_weight="0.443"
                app:layout_constraintStart_toStartOf="@+id/guideline4"
                app:layout_constraintTop_toTopOf="parent">

                <androidx.constraintlayout.widget.ConstraintLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:padding="5dp">

                    <ImageView
                        android:scaleType="centerInside"
                        android:id="@+id/imageView4"
                        android:layout_width="0dp"
                        android:layout_height="0dp"
                        android:layout_marginHorizontal="12dp"
                        android:layout_marginTop="5dp"
                        android:src="@drawable/car"
                        app:layout_constraintDimensionRatio="H,1:1"
                        app:layout_constraintEnd_toEndOf="parent"
                        app:layout_constraintStart_toStartOf="parent"
                        app:layout_constraintTop_toTopOf="parent" />

                    <TextView
                        android:id="@+id/textView7"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/home_have_text1"
                        android:textColor="#4ca730"
                        android:textSize="6sp"
                        app:layout_constraintBottom_toBottomOf="@+id/imageView4"
                        app:layout_constraintEnd_toEndOf="@+id/imageView4"
                        app:layout_constraintStart_toStartOf="@+id/imageView4"
                        tools:ignore="SmallSp" />

                    <Button
                        android:id="@+id/vehicleInfoemation_btn"
                        android:layout_width="0dp"
                        android:layout_height="0dp"
                        android:layout_marginHorizontal="2dp"
                        android:background="@drawable/blue2"
                        android:text="@string/home_have_text2"
                        android:textColor="@color/white"
                        android:textSize="8sp"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintDimensionRatio="H,24:4"
                        app:layout_constraintEnd_toEndOf="@+id/imageView4"
                        app:layout_constraintStart_toStartOf="@+id/imageView4"
                        app:layout_constraintTop_toBottomOf="@+id/imageView4"
                        tools:ignore="SmallSp" />
                </androidx.constraintlayout.widget.ConstraintLayout>
            </androidx.cardview.widget.CardView>

            <androidx.cardview.widget.CardView
                android:id="@+id/cardView2"
                android:layout_width="0dp"
                android:layout_height="0dp"
                android:layout_marginStart="10dp"
                android:background="@color/white"
                android:foreground="@drawable/card_view_selector"
                app:cardCornerRadius="15dp"
                app:layout_constraintBottom_toTopOf="@+id/cardView3"
                app:layout_constraintEnd_toStartOf="@+id/guideline3"
                app:layout_constraintHorizontal_weight="0.566"
                app:layout_constraintStart_toEndOf="@+id/constraintLayout"
                app:layout_constraintTop_toTopOf="@+id/constraintLayout"
                app:layout_constraintVertical_chainStyle="packed">

                <androidx.constraintlayout.widget.ConstraintLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent">

                    <ImageView
                        android:scaleType="centerInside"
                        android:id="@+id/imageView7"
                        android:layout_width="0dp"
                        android:layout_height="0dp"
                        android:layout_marginVertical="7dp"
                        android:src="@drawable/alcohol_monitoring"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintDimensionRatio="W,1:1"
                        app:layout_constraintEnd_toStartOf="@+id/textView12"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintStart_toStartOf="parent"
                        app:layout_constraintTop_toTopOf="parent" />

                    <TextView
                        android:id="@+id/textView12"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/home_have_text3"
                        android:textColor="#161616"
                        android:textSize="8sp"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintEnd_toEndOf="parent"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintStart_toEndOf="@+id/imageView7"
                        app:layout_constraintTop_toTopOf="parent"
                        tools:ignore="SmallSp" />
                </androidx.constraintlayout.widget.ConstraintLayout>
            </androidx.cardview.widget.CardView>

            <androidx.cardview.widget.CardView
                android:id="@+id/cardView3"
                android:layout_width="0dp"
                android:layout_height="0dp"
                android:layout_marginTop="10dp"
                android:background="@color/white"
                android:foreground="@drawable/card_view_selector"
                app:cardCornerRadius="15dp"
                app:layout_constraintBottom_toBottomOf="@+id/constraintLayout"
                app:layout_constraintEnd_toEndOf="@+id/cardView2"
                app:layout_constraintStart_toStartOf="@+id/cardView2"
                app:layout_constraintTop_toBottomOf="@+id/cardView2">

                <androidx.constraintlayout.widget.ConstraintLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent">

                    <ImageView
                        android:scaleType="centerInside"
                        android:id="@+id/imageView8"
                        android:layout_width="0dp"
                        android:layout_height="0dp"
                        android:layout_marginVertical="7dp"
                        android:src="@drawable/behavior_monitoring"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintDimensionRatio="W,1:1"
                        app:layout_constraintEnd_toStartOf="@+id/textView13"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintStart_toStartOf="parent"
                        app:layout_constraintTop_toTopOf="parent" />

                    <TextView
                        android:id="@+id/textView13"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/home_have_text4"
                        android:textColor="#161616"
                        android:textSize="8sp"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintEnd_toEndOf="parent"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintStart_toEndOf="@+id/imageView8"
                        app:layout_constraintTop_toTopOf="parent"
                        tools:ignore="SmallSp" />
                </androidx.constraintlayout.widget.ConstraintLayout>
            </androidx.cardview.widget.CardView>

            <androidx.cardview.widget.CardView
                android:layout_width="0dp"
                android:layout_height="0dp"
                android:layout_marginTop="37dp"
                android:background="@color/white"
                app:cardCornerRadius="30dp"
                app:layout_constraintDimensionRatio="W,3:10"
                app:layout_constraintEnd_toStartOf="@+id/guideline3"
                app:layout_constraintStart_toStartOf="@+id/guideline4"
                app:layout_constraintTop_toBottomOf="@+id/constraintLayout">

                <androidx.constraintlayout.widget.ConstraintLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    tools:ignore="SmallSp">

                    <androidx.constraintlayout.widget.ConstraintLayout
                        android:id="@+id/constraintLayout4"
                        android:layout_width="wrap_content"
                        android:layout_height="match_parent"
                        app:layout_constraintEnd_toStartOf="@+id/physicalIndicators_btn"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintHorizontal_chainStyle="packed"
                        app:layout_constraintStart_toStartOf="parent">


                        <ImageView
                            android:scaleType="centerInside"
                            android:id="@+id/imageView9"
                            android:layout_width="0dp"
                            android:layout_height="0dp"
                            android:layout_marginVertical="11dp"
                            android:src="@drawable/smart_bracelet"
                            app:layout_constraintBottom_toBottomOf="parent"
                            app:layout_constraintDimensionRatio="H,1:1"
                            app:layout_constraintStart_toStartOf="parent"
                            app:layout_constraintTop_toTopOf="parent" />

                        <TextView
                            android:id="@+id/textView10"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="@string/home_have_text5"
                            android:textColor="#383838"
                            android:textSize="12sp"
                            app:layout_constraintBottom_toTopOf="@+id/textView11"
                            app:layout_constraintStart_toEndOf="@+id/imageView9"
                            app:layout_constraintTop_toTopOf="parent"
                            app:layout_constraintVertical_chainStyle="packed" />

                        <TextView
                            android:id="@+id/textView11"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:layout_marginVertical="5dp"
                            android:text="@string/home_have_text6"
                            android:textColor="#888888"
                            android:textSize="6sp"
                            app:layout_constraintBottom_toTopOf="@+id/textView9"
                            app:layout_constraintStart_toEndOf="@+id/imageView9"
                            app:layout_constraintTop_toBottomOf="@+id/textView10" />

                        <TextView
                            android:id="@+id/textView9"
                            android:layout_width="wrap_content"
                            android:layout_height="wrap_content"
                            android:text="@string/home_have_text7"
                            android:textColor="#4ca730"
                            android:textSize="10sp"
                            app:layout_constraintBottom_toBottomOf="parent"
                            app:layout_constraintStart_toEndOf="@+id/imageView9"
                            app:layout_constraintTop_toBottomOf="@+id/textView11" />
                    </androidx.constraintlayout.widget.ConstraintLayout>

                    <Button
                        android:id="@+id/physicalIndicators_btn"
                        android:layout_width="0dp"
                        android:layout_height="20dp"
                        android:layout_marginStart="15dp"
                        android:background="@drawable/blue2"
                        android:text="@string/home_have_text8"
                        android:textColor="@color/white"
                        android:textSize="8sp"
                        app:layout_constraintBottom_toBottomOf="parent"
                        app:layout_constraintDimensionRatio="W,16:4"
                        app:layout_constraintEnd_toEndOf="parent"
                        app:layout_constraintHorizontal_bias="0.5"
                        app:layout_constraintStart_toEndOf="@+id/constraintLayout4"
                        app:layout_constraintTop_toTopOf="parent" />
                </androidx.constraintlayout.widget.ConstraintLayout>
            </androidx.cardview.widget.CardView>
        </androidx.constraintlayout.widget.ConstraintLayout>

        <androidx.constraintlayout.widget.ConstraintLayout
            android:id="@+id/blank_layout"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/header_imageview">

            <ImageView
                android:scaleType="centerInside"
                android:id="@+id/imageView2"
                android:layout_width="131dp"
                android:layout_height="131dp"
                android:layout_marginTop="60dp"
                android:src="@drawable/blank"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="parent" />

            <Button
                android:id="@+id/add_device_btn"
                android:layout_width="105dp"
                android:layout_height="32dp"
                android:layout_marginTop="25dp"
                android:background="@drawable/blue_radius11_btn"
                android:text="@string/add_device_btn"
                android:textColor="@color/white"
                android:textSize="12sp"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/imageView2" />
        </androidx.constraintlayout.widget.ConstraintLayout>

        <com.example.a02.CustomView.StatusBarHightView
            android:id="@+id/statusBarView_home"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            app:layout_constraintTop_toTopOf="parent" />

        <de.hdodenhof.circleimageview.CircleImageView
            android:id="@+id/header_imageview"
            android:layout_width="36dp"
            android:layout_height="36dp"
            android:scaleType="centerCrop"
            android:src="@drawable/default_header"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintStart_toStartOf="@+id/guideline1"
            app:layout_constraintTop_toBottomOf="@+id/statusBarView_home"
            app:layout_constraintVertical_bias="0.03" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="48dp"
            android:layout_height="18dp"
            android:layout_marginStart="10dp"
            android:background="@drawable/device_status_tv"
            android:gravity="center"
            android:text="@string/device_status1"
            android:textColor="@color/white"
            android:textSize="9sp"
            app:layout_constraintBottom_toBottomOf="@+id/header_imageview"
            app:layout_constraintStart_toEndOf="@+id/header_imageview"
            app:layout_constraintTop_toTopOf="@+id/header_imageview"
            tools:ignore="SmallSp" />

        <Button
            android:scaleType="centerInside"
            android:id="@+id/add_imageButton"
            android:layout_width="25dp"
            android:layout_height="25dp"
            android:background="@drawable/add_selector"
            app:layout_constraintBottom_toBottomOf="@+id/header_imageview"
            app:layout_constraintEnd_toStartOf="@+id/guideline2"
            app:layout_constraintTop_toTopOf="@+id/header_imageview" />

        <Button
            android:scaleType="centerInside"
            android:id="@+id/message_imageButton"
            android:layout_width="20dp"
            android:layout_height="25dp"
            android:layout_marginEnd="12dp"
            android:background="@drawable/message"
            app:layout_constraintBottom_toBottomOf="@+id/header_imageview"
            app:layout_constraintEnd_toStartOf="@+id/add_imageButton"
            app:layout_constraintTop_toTopOf="@+id/header_imageview" />

        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guideline2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            app:layout_constraintGuide_percent="0.9" />
        <androidx.constraintlayout.widget.Guideline
            android:id="@+id/guideline1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            app:layout_constraintGuide_percent="0.1" />
    </androidx.constraintlayout.widget.ConstraintLayout>

</FrameLayou
```
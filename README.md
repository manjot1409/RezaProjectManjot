ViewAnimator
=======

[![API](https://img.shields.io/badge/API-11%2B-green.svg)](https://github.com/florent37/ViewAnimator/tree/master)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-ViewAnimator-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/2942)

A fluent Android animation library !


<a href="https://goo.gl/WXW8Dc">
  <img alt="Android app on Google Play" src="https://developer.android.com/images/brand/en_app_rgb_wo_45.png" />
</a>

[![png](https://raw.githubusercontent.com/florent37/ViewAnimator/master/montain_small.jpg)](https://github.com/florent37/ViewAnimator)

# Usage

Animate multiple view from one method

```java
ViewAnimator
       .animate(image)
            .translationY(-1000, 0)
            .alpha(0,1)
       .andAnimate(text)
            .dp().translationX(-20, 0)
            .decelerate()
            .duration(2000)
       .thenAnimate(image)
            .scale(1f, 0.5f, 1f)
            .accelerate()
            .duration(1000)
       .start();
       
```

[![gif](https://j.gifs.com/ERlBzW.gif)](https://youtu.be/ZHw8MfOM1Eg)

Without ViewAnimator

```java
AnimatorSet animatorSet = new AnimatorSet();
animatorSet.playTogether(
  ObjectAnimator.ofFloat(image,"translationY",-1000,0),
  ObjectAnimator.ofFloat(image,"alpha",0,1),
  ObjectAnimator.ofFloat(text,"translationX",-200,0)
);
animatorSet.setInterpolator(new DescelerateInterpolator());
animatorSet.setDuration(2000);
animatorSet.addListener(new AnimatorListenerAdapter(){
    @Override public void onAnimationEnd(Animator animation) {

      AnimatorSet animatorSet2 = new AnimatorSet();
      animatorSet2.playTogether(
          ObjectAnimator.ofFloat(image,"scaleX", 1f, 0.5f, 1f),
          ObjectAnimator.ofFloat(image,"scaleY", 1f, 0.5f, 1f)
      );
      animatorSet2.setInterpolator(new AccelerateInterpolator());
      animatorSet2.setDuration(1000);
      animatorSet2.start();

    }
});
animatorSet.start();
```

# More

[![gif](https://j.gifs.com/XD6R4V.gif)](https://youtu.be/Qlj40Y6ChSM)

Add same animation on multiples view
```java
ViewAnimator
       .animate(image,text)
       .scale(0,1)
       .start();
```

Add listeners
```java
ViewAnimator
       .animate(image)
       .scale(0,1)
       .onStart(() -> {})
       .onStop(() -> {})
       .start();

```

Use DP values
```java
ViewAnimator
       .animate(image)
       .dp().translationY(-200, 0)
       .start();
```

Animate Height / Width
```java
ViewAnimator
       .animate(view)
       .waitForHeight() //wait until a ViewTreeObserver notification
       .dp().width(100,200)
       .dp().height(50,100)
       .start();
```

Color animations
```java
ViewAnimator
       .animate(view)
       .textColor(Color.BLACK,Color.GREEN)
       .backgroundColor(Color.WHITE,Color.BLACK)
       .start();
```

Rotation animations
```java
ViewAnimator
       .animate(view)
       .rotation(360)
       .start();
```

Custom animations
```java
ViewAnimator
       .animate(text)
       .custom(new AnimationListener.Update<TextView>() {
            @Override public void update(TextView view, float value) {
                  view.setText(String.format("%.02f",value));
            }
        }, 0, 1)
       .start();
```

Cancel animations
```java
ViewAnimator viewAnimator = ViewAnimator
       .animate(view)
       .rotation(360)
       .start();

viewAnimator.cancel();
```

Enhanced animations (Thanks [AndroidViewAnimations](https://github.com/daimajia/AndroidViewAnimations) and [NiftyDialogEffects](https://github.com/sd6352051/NiftyDialogEffects) )   
```java
.shake().interpolator(new LinearInterpolator());
.bounceIn().interpolator(new BounceInterpolator());
.flash().repeatCount(4);
.flipHorizontal();
.wave().duration(5000);
.tada();
.pulse();
.standUp();
.swing();
.wobble();
```
...
![Preview](/EnhancedAnimations.gif)

Path animations ( Inspiration from http://blog.csdn.net/tianjian4592/article/details/47067161 )   
```java
    final int[] START_POINT = new int[]{ 300, 270 };
    final int[] BOTTOM_POINT = new int[]{ 300, 400 };
    final int[] LEFT_CONTROL_POINT = new int[]{ 450, 200 };
    final int[] RIGHT_CONTROL_POINT = new int[]{ 150, 200 };
    Path path = new Path();
    path.moveTo(START_POINT[0], START_POINT[1]);
    path.quadTo(RIGHT_CONTROL_POINT[0], RIGHT_CONTROL_POINT[1], BOTTOM_POINT[0], BOTTOM_POINT[1]);
    path.quadTo(LEFT_CONTROL_POINT[0], LEFT_CONTROL_POINT[1], START_POINT[0], START_POINT[1]);
    path.close();
    ViewAnimator.animate(view).path(path).repeatCount(-1).start();
```
Java Code
package com.example.kks.rezaprojectmanjot;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.view.animation.AccelerateDecelerateInterpolator;
import android.view.animation.RotateAnimation;
import android.widget.Button;
import android.widget.ImageView;

import java.util.Random;

public class MainActivity extends AppCompatActivity {
    ImageView imageButton2;
    Button button;

    Random random;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        random = new Random();
        imageButton2 = (ImageView) findViewById(R.id.imageButton2);
        button = (Button) findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                RotateAnimation rotateAnimation = new RotateAnimation(0,random.nextInt(3600) + 360, RotateAnimation.RELATIVE_TO_SELF, 0.5f, RotateAnimation.RELATIVE_TO_SELF, 0.5f);
                rotateAnimation.setFillAfter(true);
                rotateAnimation.setDuration(500);
                rotateAnimation.setInterpolator(new AccelerateDecelerateInterpolator());

                imageButton2.startAnimation(rotateAnimation);
            }
        });
    }
}


```





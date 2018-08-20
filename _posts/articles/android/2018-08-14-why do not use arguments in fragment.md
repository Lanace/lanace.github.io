---
layout: article
title: "왜 Fragment는 기본 생성자를 사용해야만 하는가?"
categories: articles
modified: 2018-08-14
tags: [android, fragment]
comments: true
ads: true
image:
  feature: android_common.jpg
  teaser: android_common.jpg
  thumb: android_common.jpg
---

## 문제

**Error message:** android.support.v4.app.Fragment$InstantiationException: Unable to instantiate fragment 
make sure class name exists, is public, and has an empty constructor that is public
{: .notice-danger}

> android.support.v4.app.Fragment$InstantiationException: Unable to instantiate fragment 
> make sure class name exists, is public, and has an empty constructor that is public

위에 코드는 `Fragment`를 사용할 때 종종 보게되는 에러이다. 
이 에러는 항상 발생하는것이 아니다. 가끔, 아주 가끔 발생한다. 이러한 종류의 에러가 가장 싫다... (~~내가 제일 싫어하는 버그ㅠ~~)

하지만 의외로 해결 방법은 간단하다. 
[가이드](https://developer.android.com/reference/android/app/Fragment.html)에 나와있는것처럼 코드를 작성하면 쉽게 해결된다.

``` java
public static class DetailsFragment extends Fragment {
    /**
     * Create a new instance of DetailsFragment, initialized to
     * show the text at 'index'.
     */
    public static DetailsFragment newInstance(int index) {
        DetailsFragment f = new DetailsFragment();

        // Supply index input as an argument.
        Bundle args = new Bundle();
        args.putInt("index", index);
        f.setArguments(args);

        return f;
    }

    public int getShownIndex() {
        return getArguments().getInt("index", 0);
    }

     @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
            Bundle savedInstanceState) {
        TextView textView = new TextView(getActivity());
        text.setText(String.valueOf(getShownIndex()));
        return textView;
    }   
}
```

위의 코드는 아주 많이들 참고하고 있는 코드이다. 여기서의 해결방법은 생성자를 만들지 않거나, 만들더라고 아무짓도 하지 않는것이다. 
즉, 

``` java
public DetailsFragment() {
  // Do not anything!!!
}
```

이러한 기본 생성자만이 존재해야 한다는것이다.
지금까진 이렇게 작성하는것이 관래인건가... 그냥 이렇게 하면 되는구나 했지만 이젠 자세하게 알아볼 때가 된것같아서 찾아보게 되었다.

## Fragment source code

[Fragment soucrce code](https://android.googlesource.com/platform/frameworks/base/+/master/core/java/android/app/Fragment.java)를 살펴보자.
여기 있는 모든 코드를 보는것도 좋지만 우리가 필요한 코드를 찾아보자.

`instantiate()`와 `newInstance()`이다.

```java
public static Fragment instantiate(Context context, String fname, @Nullable Bundle args) {
    try {
        Class<?> clazz = sClassMap.get(fname);
        if (clazz == null) {
            // Class not found in the cache, see if it's real, and try to add it
            clazz = context.getClassLoader().loadClass(fname);
            if (!Fragment.class.isAssignableFrom(clazz)) {
                throw new InstantiationException("Trying to instantiate a class " + fname
                        + " that is not a Fragment", new ClassCastException());
            }
            sClassMap.put(fname, clazz);
        }
        Fragment f = (Fragment) clazz.getConstructor().newInstance();
        if (args != null) {
            args.setClassLoader(f.getClass().getClassLoader());
            f.setArguments(args);
        }
        return f;
    } catch (ClassNotFoundException e) {
        throw new InstantiationException("Unable to instantiate fragment " + fname
                + ": make sure class name exists, is public, and has an"
                + " empty constructor that is public", e);
    } catch (java.lang.InstantiationException e) {
        throw new InstantiationException("Unable to instantiate fragment " + fname
                + ": make sure class name exists, is public, and has an"
                + " empty constructor that is public", e);
    } catch (IllegalAccessException e) {
        throw new InstantiationException("Unable to instantiate fragment " + fname
                + ": make sure class name exists, is public, and has an"
                + " empty constructor that is public", e);
    } catch (NoSuchMethodException e) {
        throw new InstantiationException("Unable to instantiate fragment " + fname
                + ": could not find Fragment constructor", e);
    } catch (InvocationTargetException e) {
        throw new InstantiationException("Unable to instantiate fragment " + fname
                + ": calling Fragment constructor caused an exception", e);
    }
}
```

[이 Document](https://docs.oracle.com/javase/6/docs/api/java/lang/Class.html#newInstance())에서 설명하고 있는것을 살펴보자.

> Creates a new instance of the class represented by this Class object. The class is instantiated as if by a new expression with an empty argument list. The class is initialized if it has not already been initialized.

즉, Class의 새로운 instance를 생성하는데, 이는 `new`를 사용하여 인자가 없는 생성자로 생성하고, 초기화 된적이 없을때 새로 초기화 한다는 의미다.

`instantiation`이 public 접근자를 체크하고, `class loader`가 접근 가능한지는 확인한다.

```java
Fragment f = (Fragment) clazz.getConstructor().newInstance();
```

그렇다면 이 코드에서는 기본 생성자를 가져와 Fragment를 생성한다는 의미인데, 

이 방법은 그닥 좋은 방법이 아닌것 같지만, `FragmentManger`가 `Fragments`를 죽이고 재생성하는것을 가능하게 해준다.
~~(androic에서 Activity가 이것과 유사한 방법으로 되어있다고 한다...)~~


## 이렇게 구현된 이유

자 그렇다면 왜 이렇게 구현을 한것일까. 의도가 무엇인지가 중요하다.

프로그먼트를 만들 때는 생성자를 오버로딩 하지 않고 생성 시 파라미터를 Bundle에 담아 setArgument() 함수를 호출하는 방식을 사용하는 것이 일반적입니다. 왜냐하면 안드로이드에 의해서 프래그먼트가 복원될 때는 프래그먼트의 기본 생성자를 호출하기 때문에 오버로딩된 생성자의 호출이 보장되지 않습니다.

## 적절한 해결 방법

자 그래서 어떻게 하면 가장 잘(?) 이러한 문제를 피해갈 수 있을까. 
가장 좋은 방법은 `Fragment`에서 기본생성자 외에 절대 사용하지 않는것이다.

그렇다면 파라미터는 어떻게 넘기는가?
아래 방법을 잘 활용해보자.

```java
public static final MyFragment newInstance(int title, String message) {
    MyFragment f = new MyFragment();
    Bundle bdl = new Bundle(2);
    bdl.putInt(EXTRA_TITLE, title);
    bdl.putString(EXTRA_MESSAGE, message);
    f.setArguments(bdl);
    return f;
}

@Override
public void onCreate(Bundle savedInstanceState) {
    title = getArguments().getInt(EXTRA_TITLE);
    message = getArguments().getString(EXTRA_MESSAGE);

    //...
    //etc
    //...
}
```


<!-- 

######################### https://stackoverflow.com/a/25984130/5949460

is empty constructor really required?

Yes.

Why does it work when orientation does not change?

Because Android is not trying to recreate your fragments.

Why does it fail when orientation is changed?

Because Android is recreating your fragments.

When a configuration change occurs (e.g., orientation change), by default Android destroys and recreates your activity, and also destroys and recreates the fragments in that activity. The "recreates the fragments" part is why you need the zero-argument public constructor on your fragments. It is also used in other cases, such as with a FragmentStatePagerAdapter.

Or, to quote the documentation:

All subclasses of Fragment must include a public empty constructor. The framework will often re-instantiate a fragment class when needed, in particular during state restore, and needs to be able to find this constructor to instantiate it. If the empty constructor is not available, a runtime exception will occur in some cases during state restore.
-->
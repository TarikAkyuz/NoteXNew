This blog shows you how to set up and work with Navigation Component.
To include Navigation suppport in your project, add the following dependencies to your app's <code>build.gradle</code> file:

```Groovy
dependencies {
  // Kotlin
  implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
  implementation "androidx.navigation:navigation-ui-ktx:$nav_version"
}
```

## Ensure type-safety by using Safe Args

The recommended way to navigate between destinations is to use the Safe Args Gradle plugin. This plugin generates simple object and builder classes that enable type-safe navigation and argument passing between destinations.

To add <code>Safe Args</code> to your project, include to following classpath in your top level build.gradle file:

```Groovy
buildscript {
    ext.nav_version = "2.3.5"
    repositories {
        google()
    }
    dependencies {
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
    }
}
```

Also include to the following plugin in your your app <code>build.gradle</code> file:

```Groovy
plugins {
  id 'androidx.navigation.safeargs'
}
```
You must have <code>android.useAndroidX=true</code> in your gradle.properties file as per Migrating to AndroidX.


## Add a NavHost to an activity

One of the core parts of the Navigation component is the navigation host. The navigation host is an empty container where destinations are swapped in and out as a user navigates through your app.

A navigation host must derive from <code>NavHost</code>. The Navigation component's default NavHost implementation, NavHostFragment, handles swapping fragment destinations.

> **Note**: The Navigation component is designed for apps that have one main activity with multiple fragment destinations. The main activity is associated with a navigation graph and contains a NavHostFragment that is responsible for swapping destinations as needed. In an app with multiple activity destinations, each activity has its own navigation graph.


```XML
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.appcompat.widget.Toolbar
        .../>

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"

        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <com.google.android.material.bottomnavigation.BottomNavigationView
        .../>

</androidx.constraintlayout.widget.ConstraintLayout>
```

Note the following:

- The <code>android:name</code> attribute contains the class name of your <code>NavHost</code> implementation.
- The <code>app:navGraph</code> attribute associates the <code>NavHostFragment</code> with a navigation graph. The navigation graph specifies all of the destinations in this NavHostFragment to which users can navigate.
- The <code>app:defaultNavHost= "true"</code> attribute ensures that your NavHostFragment intercepts the system back button. Note that only one <code>NavHost</code> can be the default.


In code:
- As an example, assume we have a navigation graph with a single action that connects the originating destination, <code>SpecifyAmountFragment</code>, to a receiving destination, <code>ConfirmationFragment</code>.
- Safe Args generates a <code>SpecifyAmountFragmentDirections</code> class with a single method, <code>actionSpecifyAmountFragmentToConfirmationFragment()</code> that returns a <code>NavDirections</code> object. This returned <code>NavDirections</code> object can then be passed directly to <code>navigate()</code>, as shown in the following example:

```Kotlin
override fun onClick(view: View) {
    //SpecifyAmountFragmentDirections created by safe args
    val action =
        SpecifyAmountFragmentDirections
            .actionSpecifyAmountFragmentToConfirmationFragment()
    view.findNavController().navigate(action)
}
```

6:50 #4
# Android Navigation App
### Hello and welcome
This app is a demonstration on how navigation works, fragments and databinding. Let's talk steps

1. Go ahead create a fragment, named TitleFragment, its xml will be named fragment_title
2. Clean up the code in TitleFragment, leaving only the onCreateView function
3. Within onCreateView, go ahead leave this ```        val binding: FragmentTitleBinding = DataBindingUtil.inflate(
                inflater, R.layout.fragment_title, container, false
        )
        return binding.root
        ```
        for data binding. Make sure to connect fragment_title by using R.layout, not R.id like I did earlier and causing myself a headache
4. Find the activity_main xml, and inside of its linear/constraint layout make a fragment. It's easier to drag a fragment in the component tree rather than coding it
because Android will conveniently ask you which fragment you are inserting. Go ahead and choose TitleFragment. If you were to hardcode this, you'd need to include this
line ```android:name="com.example.android.navigation.TitleFragment"``` within <fragment /> for Android to know which fragment you want to use. Give it match parent height and
width, and give it whatever ID you want.


Well done, now you have made a fragment pop up within an activity, which is a good practise

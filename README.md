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
        for data binding

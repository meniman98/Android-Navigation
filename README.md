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

### How to add Navigation
1. Go ahead make the navigation dir within the res dir. Then right click on the navigation dir (within the res) and make a new navigation xml file. Name it navigation. Now 
to reference this file, use @navigation/navigation. In this file you'll be doing your navigation stuff obviously
2. Navigate to your activity main xml and find the fragment within its layout and change its name to navHostFragment like this ```android:name="androidx.navigation.fragment.NavHostFragment"``` its ID to "navHostFragment" then add these 2 attributes: 
```
app:defaultNavHost="true"
app:navGraph="@navigation/navigation"
```
3. Navigate to your navigation xml and add the two fragments by clicking the plus +  sign. On the navHost there'll be an attribute like this ``` app:startDestination="@id/titleFragment">``` and that will determine which fragment is going to be the starter one. Now while inside navigation xml, 
click on the TitleFragment and drag it to your GameFragment. This makes an action, meaning that the user goes from title to game
4. Create the navigation onclick listener. Navigate to your navHost (TitleFragment) and add this code 
```
       binding.playButton.setOnClickListener { view: View ->
            Navigation.findNavController(view).navigate(R.id.action_titleFragment_to_gameFragment)
        }
```
The playButton is a button on the xml layout, the R.id.action_titleFragment_to_gameFragment is your ID for the action.

There you go, you're done now. You've created two fragments that produces an action from title to game

### How to add a menu on the status bar with an "About page"
1. Firstly add the About xml layout in your navigation graph. No need for setting a path from anywhere
2. Create a dir within res named "menu" and create an xml file of type menu and name it overflow_menu
3. Go within overflow_menu xml and add in a menu item. Your menu item will be the About fragment. Add it there
4. Now your menu is created, it must appear now. Go to TitleFragment and use this code:

```
 // for your menu
        setHasOptionsMenu(true)

    // inflate the menu, make it appear
    override fun onCreateOptionsMenu(menu: Menu, inflater: MenuInflater) {
        super.onCreateOptionsMenu(menu, inflater)
        inflater.inflate(R.menu.overflow_menu, menu)
    }

    // assign the menu button an action
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return NavigationUI.onNavDestinationSelected(item,
        view!!.findNavController()) ||
        super.onOptionsItemSelected(item)
    }
```

### How to add produce an intent on a menu item click
In the game won fragment, a share button has been added on the right corner. Upon clicking it, a share intent is triggered.
To replicate this, do the following:
1. Produce the function for the share intent like this

```
    // Getting the share intent
    private fun getShareIntent(): Intent {
        val args = GameWonFragmentArgs.fromBundle(arguments!!)
        val shareIntent = Intent(Intent.ACTION_SEND)
        shareIntent.setType("text/plain")
                .putExtra(Intent.EXTRA_TEXT, getString(
                        R.string.share_success_text,
                        args.numQuestions,
                        args.numCorrect
                ))
        return shareIntent
    }
```

2. Now add the execution function which provides an alert dialog if the intent failed

```
    // execute the share function
    private fun shareSuccess() {
        // if the intent doesn't resolve correctly
        if (null == getShareIntent().resolveActivity(activity!!.packageManager)) {
            showAlertDialog()
        } else {
            startActivity(getShareIntent())
        }
    }
```

3. Finally add the click listener like so:

```
// Sharing from the Menu
override fun onOptionsItemSelected(item: MenuItem?): Boolean {
   when (item!!.itemId) {
       R.id.share -> shareSuccess()
   }
   return super.onOptionsItemSelected(item)
}
```
That's Gucci dawg well done

### Errors and mistakes to look out for
1. I made the mistake with the popping feature by clicking on the game won fragment. Instead I must click on the game fragment instead, choose a pop behaviour, and set it to
title fragment non inclusive. If set to inclusive, it will pop title fragment along with it. Alternatively, you can set the pop behaviour to game fragment then tick the box
for inclusive. That would also return you to the title fragment when pressing the back button

2. This has to be the funniest mistake I made. Just look at this code and tell me what's wrong

```

    // execute the share function
    private fun shareSuccess() {}
    override fun startActivity(intent: Intent?) {
        startActivity(getShareIntent())
    }
```

When I clicked the share button and saw it didn't react, I
inspected every line closely just to deadass find this oh goodness.
The function body is not wrapping around the logic. I lost it when I saw this.
Laughed too hard.

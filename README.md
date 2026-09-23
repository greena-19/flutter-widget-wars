Team Name : Greena and Anuradha 

State Architecture Review: setState() vs. ValueNotifier

For our Superhero Deck application, I used setState() to manage the mission counter, energy level, status message, theme, and button press effects. I think setState() is a good choice for the current version because the app is small and its interactions are simple. The state is easy to find: the main screen owns the mission count, energy level, and status, while each power button owns its own isPressed value. When a value changes, calling setState() tells Flutter to rebuild the relevant widget.

If the app grows, I would consider using ValueNotifier for values such as the energy level or mission count. A ValueNotifier holds a value and notifies its listeners when that value changes. Instead of keeping the energy level directly inside the screen’s state, I could store it in a separate notifier and let the widgets that display energy listen to it.

The main difference is rebuild scope. With the current setState() approach, changing the slider rebuilds the main screen’s widget subtree, including widgets that do not depend on energy. With ValueNotifier and ValueListenableBuilder, I could rebuild only the energy display when the energy value changes. The four power buttons would not need to rebuild just because I moved the slider. This separation would become more useful if I added more controls, statistics, and dashboard sections.

ValueNotifier could also improve testability. I could test whether changing the energy value notifies listeners without running the entire app or interacting with the slider. With the current approach, testing the complete interaction is more closely tied to the screen and its widgets. However, I would still keep setState() for each button’s temporary pressed effect because that state is local to one button and does not need to be shared.

One situation where ValueNotifier would be a better fit is a larger Superhero Deck with an energy display in multiple places, such as the dashboard, a mission screen, and a power-control panel. All three could listen to the same energy notifier and stay updated. For the current assignment, setState() remains simple and appropriate, but ValueNotifier would help organize shared state as the application expands.

Source: Flutter API documentation — ValueNotifier 

Pseudocode: Energy Level with ValueNotifier
final energyLevel = ValueNotifier<double>(65.0);

Slider(
  value: energyLevel.value,
  onChanged: (newValue) => energyLevel.value = newValue,
);

ValueListenableBuilder<double>(
  valueListenable: energyLevel,
  builder: (context, value, child) => Text('${value.toInt()}%'),
);
```tsx
import { View } from "react-native";
import {
Gesture,
GestureDetector,
Directions,
} from "react-native-gesture-handler";
import { useNavigation } from "expo-router";
import { runOnJS } from "react-native-reanimated";
import { JobCreateForm } from "~/components/explore/jobs/JobCreateForm";

export default function Screen() {
	const navigate = useNavigation();
	const handleSwipeBack = () => {
		try {
			if (navigate.canGoBack()) navigate.goBack();
			else console.warn("No route to go back to");
		} catch (error) {
			console.error("Error during navigation:", error);
		}
	};
	
	const swipe = Gesture.Fling()
		.direction(Directions.RIGHT)
		.onEnd(() => {
			runOnJS(handleSwipeBack)();
		});

	return (
		<GestureDetector gesture={swipe}>
			<View style={{ flex: 1 }}>
				<JobCreateForm />
			</View>
		</GestureDetector>
	);
}
```
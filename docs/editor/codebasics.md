// navigation/AppNavigator.js
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Icon } from 'react-native-elements';

import LoginScreen from '../screens/LoginScreen';
import SignupScreen from '../screens/SignupScreen';
import HomeScreen from '../screens/HomeScreen';
import RecipeScreen from '../screens/RecipeScreen';
import NotesScreen from '../screens/NotesScreen';
import GroceryListScreen from '../screens/GroceryListScreen';
import FavoritesScreen from '../screens/FavoritesScreen';
import ChatScreen from '../screens/ChatScreen';
import NutritionScreen from '../screens/NutritionScreen';

const Stack = createStackNavigator();
const Tab = createBottomTabNavigator();

function HomeTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} options={{ tabBarIcon: ({ color, size }) => (<Icon name="home" color={color} size={size} />)}}/>
      <Tab.Screen name="Notes" component={NotesScreen} options={{ tabBarIcon: ({ color, size }) => (<Icon name="note" color={color} size={size} />)}}/>
      <Tab.Screen name="GroceryList" component={GroceryListScreen} options={{ tabBarIcon: ({ color, size }) => (<Icon name="list" color={color} size={size} />)}}/>
      <Tab.Screen name="Favorites" component={FavoritesScreen} options={{ tabBarIcon: ({ color, size }) => (<Icon name="favorite" color={color} size={size} />)}}/>
      <Tab.Screen name="Chat" component={ChatScreen} options={{ tabBarIcon: ({ color, size }) => (<Icon name="chat" color={color} size={size} />)}}/>
    </Tab.Navigator>
  );
}

export default function AppNavigator() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Signup" component={SignupScreen} />
        <Stack.Screen name="HomeTabs" component={HomeTabs} />
        <Stack.Screen name="Recipe" component={RecipeScreen} />
        <Stack.Screen name="Nutrition" component={NutritionScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
// screens/LoginScreen.js
import React, { useState } from 'react';
import { View, Text, TextInput, Button, StyleSheet } from 'react-native';
import auth from '@react-native-firebase/auth';

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const login = async () => {
    try {
      await auth().signInWithEmailAndPassword(email, password);
      navigation.replace('HomeTabs');
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <View style={styles.container}>
      <TextInput style={styles.input} placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput style={styles.input} placeholder="Password" secureTextEntry value={password} onChangeText={setPassword} />
      <Button title="Login" onPress={login} />
      <Button title="Sign Up" onPress={() => navigation.navigate('Signup')} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    padding: 16,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 12,
    padding: 8,
  },
});
// screens/SignupScreen.js
import React, { useState } from 'react';
import { View, TextInput, Button, StyleSheet } from 'react-native';
import auth from '@react-native-firebase/auth';

export default function SignupScreen({ navigation }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');

  const signup = async () => {
    try {
      await auth().createUserWithEmailAndPassword(email, password);
      navigation.replace('HomeTabs');
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <View style={styles.container}>
      <TextInput style={styles.input} placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput style={styles.input} placeholder="Password" secureTextEntry value={password} onChangeText={setPassword} />
      <Button title="Sign Up" onPress={signup} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    padding: 16,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 12,
    padding: 8,
  },
});
// screens/HomeScreen.js
import React, { useState } from 'react';
import { View, TextInput, Button, FlatList, Text, StyleSheet } from 'react-native';

export default function HomeScreen({ navigation }) {
  const [query, setQuery] = useState('');
  const [recipes, setRecipes] = useState(mockRecipes); // Replace with actual data fetching

  const searchRecipes = () => {
    // Implement search functionality
  };

  return (
    <View style={styles.container}>
      <TextInput style={styles.input} placeholder="Search Recipes" value={query} onChangeText={setQuery} />
      <Button title="Search" onPress={searchRecipes} />
      <FlatList
        data={recipes}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.recipeItem}>
            <Text style={styles.title}>{item.title}</Text>
            <Text>{item.time} minutes</Text>
            <Button title="View Recipe" onPress={() => navigation.navigate('Recipe', { recipeId: item.id })} />
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 12,
    padding: 8,
  },
  recipeItem: {
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold',
  },
});

const mockRecipes = [
  { id: '1', title: 'Salad', time: 10 },
  { id: '2', title: 'Soup', time: 20 },
  // Add more mock recipes
];
// screens/RecipeScreen.js
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export default function RecipeScreen({ route }) {
  const { recipeId } = route.params;
  const recipe = mockRecipes.find(r => r.id === recipeId); // Replace with actual data fetching

  return (
    <View style={styles.container}>
      <Text style={styles.title}>{recipe.title}</Text>
      <Text>{recipe.time} minutes</Text>
      <Text>Step-by-step instructions:</Text>
      {recipe.steps.map((step, index) => (
        <Text key={index}>{step}</Text>
      ))}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
  },
});

const mockRecipes = [
  { id: '1', title: 'Salad', time: 10, steps: ['Step 1', 'Step 2'] },
  { id: '2', title: 'Soup', time: 20, steps: ['Step 1', 'Step 2', 'Step 3'] },
  // Add more mock recipes
];
// screens/NotesScreen.js
import React, { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, StyleSheet } from 'react-native';

export default function NotesScreen() {
  const [note, setNote] = useState('');
  const [notes, setNotes] = useState([]);

  const addNote = () => {
    setNotes([...notes, note]);
    setNote('');
  };

  return (
    <View style={styles.container}>
      <TextInput style={styles.input} placeholder="Add Note" value={note} onChangeText={setNote} />
      <Button title="Add" onPress={addNote} />
      <FlatList
        data={notes}
        keyExtractor={(item, index) => index.toString()}
        renderItem={({ item }) => <Text style={styles.note}>{item}</Text>}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 16,
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 12,
    padding: 8,
  },
  note: {
    padding: 16,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
});

```

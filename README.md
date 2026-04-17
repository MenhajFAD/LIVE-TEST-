import 'dart:convert';
import 'package:flutter/material.dart';

// 1. Recipe Model Class
class Recipe {
  final String title;
  final String description;
  final List<String> ingredients;

  Recipe({
    required this.title,
    required this.description,
    required this.ingredients,
  });

  // JSON theke object bananor factory method
  factory Recipe.fromJson(Map<String, dynamic> json) {
    return Recipe(
      title: json['title'],
      description: json['description'],
      ingredients: List<String>.from(json['ingredients']),
    );
  }
}

void main() {
  runApp(MaterialApp(
    debugShowCheckedModeBanner: false,
    home: RecipeListScreen(),
  ));
}

class RecipeListScreen extends StatefulWidget {
  @override
  _RecipeListScreenState createState() => _RecipeListScreenState();
}

class _RecipeListScreenState extends State<RecipeListScreen> {
  // Tomar deya JSON data
  final String jsonData = '''
{
  "recipes": [
    {
      "title": "Pasta Carbonara",
      "description": "Creamy pasta dish with bacon and cheese.",
      "ingredients": ["spaghetti", "bacon", "egg", "cheese"]
    },
    {
      "title": "Caprese Salad",
      "description": "Simple and refreshing salad with tomatoes, mozzarella, and basil.",
      "ingredients": ["tomatoes", "mozzarella", "basil"]
    },
    {
      "title": "Banana Smoothie",
      "description": "Healthy and creamy smoothie with bananas and milk.",
      "ingredients": ["bananas", "milk"]
    },
    {
      "title": "Chicken Stir-Fry",
      "description": "Quick and flavorful stir-fried chicken with vegetables.",
      "ingredients": ["chicken breast", "broccoli", "carrot", "soy sauce"]
    },
    {
      "title": "Grilled Salmon",
      "description": "Delicious grilled salmon with lemon and herbs.",
      "ingredients": ["salmon fillet", "lemon", "olive oil", "dill"]
    },
    {
      "title": "Vegetable Curry",
      "description": "Spicy and aromatic vegetable curry.",
      "ingredients": ["mixed vegetables", "coconut milk", "curry powder"]
    },
    {
      "title": "Berry Parfait",
      "description": "Layered dessert with fresh berries and yogurt.",
      "ingredients": ["berries", "yogurt", "granola"]
    }
  ]
}
''';

  List<Recipe> recipes = [];

  @override
  void initState() {
    super.initState();
    loadRecipes();
  }

  // JSON data parse korar function
  void loadRecipes() {
    final Map<String, dynamic> data = jsonDecode(jsonData);
    final List<dynamic> recipeList = data['recipes'];

    setState(() {
      recipes = recipeList.map((e) => Recipe.fromJson(e)).toList();
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Recipe List"),
        centerTitle: true,
        backgroundColor: Colors.blueAccent,
      ),
      body: recipes.isEmpty
          ? Center(child: CircularProgressIndicator())
          : ListView.builder(
        padding: EdgeInsets.all(10),
        itemCount: recipes.length,
        itemBuilder: (context, index) {
          final recipe = recipes[index];
          return Card(
            elevation: 4,
            margin: EdgeInsets.only(bottom: 15),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  // Recipe Title
                  Text(
                    recipe.title,
                    style: TextStyle(
                      fontSize: 20,
                      fontWeight: FontWeight.bold,
                      color: Colors.blueAccent,
                    ),
                  ),
                  SizedBox(height: 8),
                  // Description
                  Text(
                    recipe.description,
                    style: TextStyle(
                      fontSize: 14,
                      fontStyle: FontStyle.italic,
                      color: Colors.grey[700],
                    ),
                  ),
                  Divider(height: 20),
                  // Ingredients section
                  Text(
                    "Ingredients:",
                    style: TextStyle(
                      fontWeight: FontWeight.w600,
                      fontSize: 16,
                    ),
                  ),
                  SizedBox(height: 5),
                  // List of ingredients join kore kothay dekhano
                  Text(
                    recipe.ingredients.join(", "),
                    style: TextStyle(fontSize: 14),
                  ),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}

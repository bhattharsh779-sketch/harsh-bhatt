# Flutter News App

## 📌 Project Description
This Flutter application integrates a public REST API (NewsAPI) to fetch real-time news data. The JSON response is parsed into Dart model classes and displayed using ListView.builder.

## 🎯 Objectives Achieved
- Integrated REST API using HTTP package
- Parsed JSON data into Dart model
- Displayed dynamic data using ListView
- Implemented loading and error handling

## 🛠 Technologies Used
- Flutter
- Dart
- REST API (NewsAPI)
- HTTP package

## 📷 Output
The application displays:
- News image
- Title
- Description

## 🚀 How to Run
1. Clone repository
2. Run `flutter pub get`
3. Add your API key
4. Run `flutter run`

## 👨‍💻 Author
harsh bhatt
{
  "articles": [
    {
      "title": "Breaking News",
      "description": "Something happened",
      "urlToImage": "image_url"
    }
  ]
}
import 'package:flutter/material.dart';
import '../models/news_model.dart';
import '../services/api_service.dart';

class HomeScreen extends StatefulWidget {
  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  late Future<List<Article>> futureNews;

  @override
  void initState() {
    super.initState();
    futureNews = ApiService().fetchNews();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Flutter News App"),
      ),
      body: FutureBuilder<List<Article>>(
        future: futureNews,
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            return ListView.builder(
              itemCount: snapshot.data!.length,
              itemBuilder: (context, index) {
                final article = snapshot.data![index];

                return Card(
                  margin: EdgeInsets.all(10),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      if (article.imageUrl.isNotEmpty)
                        Image.network(article.imageUrl),
                      Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: Text(
                          article.title,
                          style: TextStyle(
                              fontSize: 18, fontWeight: FontWeight.bold),
                        ),
                      ),
                      Padding(
                        padding: const EdgeInsets.all(8.0),
                        child: Text(article.description),
                      ),
                    ],
                  ),
                );
              },
            );
          } else if (snapshot.hasError) {
            return Center(child: Text("Error loading news"));
          }
          return Center(child: CircularProgressIndicator());
        },
      ),
    );
  }
}

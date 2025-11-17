# ChatCycle
// main.dart
import 'package:flutter/material.dart';

void main() {
  runApp(ChatCycleWireframeApp());
}

class ChatCycleWireframeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'ChatCycle Wireframe',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primaryColor: Colors.deepPurple,
        scaffoldBackgroundColor: Colors.white,
      ),
      home: HomeShell(),
    );
  }
}

class HomeShell extends StatefulWidget {
  @override
  _HomeShellState createState() => _HomeShellState();
}

class _HomeShellState extends State<HomeShell> {
  int _selectedIndex = 0;

  final List<Widget> _pages = [
    TrendingPage(),
    PostingPage(),
    NotificationsPage(),
    ProfilePage(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _pages[_selectedIndex],
      bottomNavigationBar: BottomAppBar(
        elevation: 8,
        shape: CircularNotchedRectangle(),
        notchMargin: 6,
        child: Container(
          height: 68,
          padding: EdgeInsets.symmetric(horizontal: 12),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Row(children: [
                _NavIcon(
                  icon: Icons.explore_outlined,
                  label: 'Explore',
                  active: _selectedIndex == 0,
                  onTap: () => setState(() => _selectedIndex = 0),
                ),
                SizedBox(width: 12),
                _NavIcon(
                  icon: Icons.chat_bubble_outline,
                  label: 'Chats',
                  active: false,
                  onTap: () {},
                ),
              ]),
              Row(children: [
                _NavIcon(
                  icon: Icons.notifications_none,
                  label: 'Notify',
                  active: _selectedIndex == 2,
                  onTap: () => setState(() => _selectedIndex = 2),
                ),
                SizedBox(width: 12),
                _NavIcon(
                  icon: Icons.person_outline,
                  label: 'Profile',
                  active: _selectedIndex == 3,
                  onTap: () => setState(() => _selectedIndex = 3),
                ),
              ]),
            ],
          ),
        ),
      ),
      floatingActionButtonLocation: FloatingActionButtonLocation.centerDocked,
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.deepPurple,
        onPressed: () => setState(() => _selectedIndex = 1),
        child: Icon(Icons.add),
      ),
    );
  }
}

class _NavIcon extends StatelessWidget {
  final IconData icon;
  final String label;
  final bool active;
  final VoidCallback onTap;

  const _NavIcon({required this.icon, required this.label, required this.onTap, this.active = false});

  @override
  Widget build(BuildContext context) {
    final color = active ? Colors.deepPurple : Colors.black54;
    return GestureDetector(
      onTap: onTap,
      child: SizedBox(
        width: 56,
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            Icon(icon, color: color),
            SizedBox(height: 4),
            Text(label, style: TextStyle(fontSize: 11, color: color)),
          ],
        ),
      ),
    );
  }
}

class PostingPage extends StatefulWidget {
  @override
  _PostingPageState createState() => _PostingPageState();
}

class _PostingPageState extends State<PostingPage> {
  String? selectedChat;
  String? selectedSubject;
  final List<String> keywords = [
    'Math', 'English', 'History',
    'Computer Science', 'Homework', 'Test', 'Quiz'
  ];
  final Set<String> selectedKeywords = {};
  final TextEditingController commentCtrl = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: SingleChildScrollView(
        padding: EdgeInsets.all(18),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(mainAxisAlignment: MainAxisAlignment.spaceBetween, children: [
              Text('Saanvi - Post', style: TextStyle(color: Colors.grey)),
              Row(children: [Icon(Icons.search), SizedBox(width: 8), Icon(Icons.notifications_outlined)])
            ]),
            SizedBox(height: 20),
            Center(
              child: Text(
                'Post Your Chat',
                style: TextStyle(fontSize: 28, fontWeight: FontWeight.bold, color: Colors.deepPurple),
              ),
            ),
            SizedBox(height: 20),

            Text("Chat", style: TextStyle(fontWeight: FontWeight.bold)),
            DropdownButtonFormField<String>(
              value: selectedChat,
              items: ['Chat 1', 'Chat 2'].map((e) => DropdownMenuItem(value: e, child: Text(e))).toList(),
              onChanged: (v) => setState(() => selectedChat = v),
              decoration: _boxDeco('Which chat would you like to post?'),
            ),

            SizedBox(height: 18),
            Text("Subject", style: TextStyle(fontWeight: FontWeight.bold)),
            DropdownButtonFormField<String>(
              value: selectedSubject,
              items: ['Calculus', 'Physics', 'Chemistry']
                  .map((e) => DropdownMenuItem(value: e, child: Text(e)))
                  .toList(),
              onChanged: (v) => setState(() => selectedSubject = v),
              decoration: _boxDeco('What subject is this?'),
            ),

            SizedBox(height: 18),
            Text("Key Words", style: TextStyle(fontWeight: FontWeight.bold)),
            Wrap(
              spacing: 8,
              runSpacing: 8,
              children: keywords.map((k) {
                final sel = selectedKeywords.contains(k);
                return ChoiceChip(
                  label: Text(k),
                  selected: sel,
                  selectedColor: Colors.deepPurple.shade100,
                  onSelected: (v) =>
                      setState(() => v ? selectedKeywords.add(k) : selectedKeywords.remove(k)),
                );
              }).toList(),
            ),

            SizedBox(height: 18),
            Text("Comment", style: TextStyle(fontWeight: FontWeight.bold)),
            Container(
              padding: EdgeInsets.all(12),
              decoration: BoxDecoration(
                border: Border.all(color: Colors.grey.shade300),
                borderRadius: BorderRadius.circular(8),
              ),
              child: TextField(
                controller: commentCtrl,
                maxLines: 6,
                decoration: InputDecoration.collapsed(hintText: 'Make a comment about your post'),
              ),
            ),

            SizedBox(height: 20),
            SizedBox(
              width: double.infinity,
              height: 50,
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(backgroundColor: Colors.deepPurple),
                onPressed: () {
                  ScaffoldMessenger.of(context)
                      .showSnackBar(SnackBar(content: Text("Posted!")));
                },
                child: Text("Post", style: TextStyle(fontSize: 16)),
              ),
            ),

            SizedBox(height: 40),
          ],
        ),
      ),
    );
  }

  InputDecoration _boxDeco(String hint) {
    return InputDecoration(
      hintText: hint,
      border: OutlineInputBorder(borderRadius: BorderRadius.circular(8)),
      contentPadding: EdgeInsets.symmetric(horizontal: 12, vertical: 14),
    );
  }
}

class TrendingPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: ListView(
        padding: EdgeInsets.all(18),
        children: [
          Row(
            children: [
              Text('Saanvi - For You', style: TextStyle(color: Colors.grey)),
              Spacer(),
              Icon(Icons.search),
              SizedBox(width: 10),
              Icon(Icons.notifications_outlined)
            ],
          ),
          SizedBox(height: 12),
          Text('Explore', style: TextStyle(fontSize: 28, fontWeight: FontWeight.bold)),
          SizedBox(height: 12),

          _questionCard(),
          SizedBox(height: 12),
          _questionCard(),

          SizedBox(height: 20),
          Text('Most Popular', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
          SizedBox(height: 12),

          ...List.generate(4, (i) => _postItem()),
        ],
      ),
    );
  }

  Widget _questionCard() {
    return Container(
      padding: EdgeInsets.all(14),
      decoration: BoxDecoration(
        border: Border.all(color: Colors.grey.shade300),
        borderRadius: BorderRadius.circular(12),
      ),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Text("Question: How to find the square root of a number?",
              style: TextStyle(fontWeight: FontWeight.bold)),
          SizedBox(height: 8),
          Text(
            "Answer: The square root of a number is a value that multiplied by itself equals...",
          ),
          SizedBox(height: 12),
          Row(
            children: [
              Icon(Icons.thumb_up_alt_outlined), SizedBox(width: 8),
              Icon(Icons.comment_outlined), SizedBox(width: 8),
              Icon(Icons.share_outlined)
            ],
          )
        ],
      ),
    );
  }

  Widget _postItem() {
    return Card(
      margin: EdgeInsets.only(bottom: 14),
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      child: ListTile(
        leading: ClipRRect(
          borderRadius: BorderRadius.circular(8),
          child: Image.asset("assets/images/post_thumb_1.jpg", width: 60, height: 60, fit: BoxFit.cover),
        ),
        title: Text("How to Build Strong Relationships", style: TextStyle(fontWeight: FontWeight.bold)),
        subtitle: Text("by Saanvi • 1 day ago"),
        trailing: Icon(Icons.more_vert),
      ),
    );
  }
}

class NotificationsPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final notes = [
      "Jane Cooper published a new article!",
      "Rachel commented on your article!",
      "Brad liked your comment!",
    ];

    return SafeArea(
      child: ListView(
        padding: EdgeInsets.all(18),
        children: [
          Text("Notifications", style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold)),
          SizedBox(height: 20),

          ...notes.map((text) => ListTile(
            leading: CircleAvatar(child: Icon(Icons.person)),
            title: Text(text),
            subtitle: Text("Today"),
          )),
        ],
      ),
    );
  }
}

class ProfilePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: ListView(
        padding: EdgeInsets.all(18),
        children: [
          Container(
            height: 160,
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(12),
              image: DecorationImage(
                image: AssetImage("assets/images/banner.jpg"),
                fit: BoxFit.cover,
              ),
            ),
            child: Align(
              alignment: Alignment.bottomLeft,
              child: Padding(
                padding: EdgeInsets.all(12),
                child: Row(
                  children: [
                    CircleAvatar(
                      radius: 30,
                      backgroundImage: AssetImage("assets/images/profile_avatar.png"),
                    ),
                    SizedBox(width: 12),
                    Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        Text("John Doe", style: TextStyle(fontSize: 16, color: Colors.white, fontWeight: FontWeight.bold)),
                        Text("description", style: TextStyle(color: Colors.white70)),
                      ],
                    ),
                    Spacer(),
                    ElevatedButton(
                      onPressed: () {},
                      style: ElevatedButton.styleFrom(backgroundColor: Colors.deepPurple),
                      child: Text("Follow"),
                    )
                  ],
                ),
              ),
            ),
          ),

          SizedBox(height: 20),
          Text("Collections", style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
          SizedBox(height: 12),

          Row(
            children: [
              Expanded(
                child: _collectionCard("TITLE 1", "assets/images/explore_card.jpg"),
              ),
              SizedBox(width: 12),
              Expanded(
                child: _collectionCard("TITLE 2", "assets/images/post_thumb_2.jpg"),
              ),
            ],
          )
        ],
      ),
    );
  }

  Widget _collectionCard(String title, String asset) {
    return Container(
      height: 130,
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(12),
        image: DecorationImage(image: AssetImage(asset), fit: BoxFit.cover),
      ),
      child: Align(
        alignment: Alignment.bottomLeft,
        child: Container(
          padding: EdgeInsets.all(8),
          decoration: BoxDecoration(
            color: Colors.black45,
            borderRadius: BorderRadius.only(
              bottomLeft: Radius.circular(12),
              bottomRight: Radius.circular(12),
            ),
          ),
          child: Text(title, style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
        ),
      ),
    );
  }
}

// pubspec.yaml
name: chatcycle_wireframe
description: ChatCycle Wireframe UI
publish_to: "none"

version: 1.0.0+1

environment:
  sdk: '>=2.18.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  uses-material-design: true

  assets:
    # Existing images
    - assets/images/banner.jpg
    - assets/images/profile_avatar.png
    - assets/images/post_thumb_1.jpg
    - assets/images/post_thumb_2.jpg
    - assets/images/explore_card.jpg

    # New assets
    - assets/profile_header.jpg
    - assets/profile_avatar.png
    - assets/collection_item_1.png
    - assets/collection_item_2.png
    - assets/explore_post_1.jpg
    - assets/explore_post_2.jpg
    - assets/explore_post_3.jpg
    - assets/popular_article_1.jpg
    - assets/popular_article_2.jpg
    - assets/popular_article_3.jpg
    - assets/popular_article_4.jpg
    - assets/popular_article_5.jpg
    - assets/popular_article_6.jpg
    - assets/user_avatar_1.png
    - assets/user_avatar_2.png
    - assets/user_avatar_3.png
    - assets/user_avatar_4.png
    - assets/user_avatar_5.png
    - assets/notif_security.png
    - assets/notif_bookmark.png
    - assets/notif_update.png
    - assets/notif_storage.png
    - assets/icon_home.png
    - assets/icon_explore.png
    - assets/icon_post.png
    - assets/icon_notifications.png
    - assets/icon_profile.png
    - assets/icon_back.png
    - assets/icon_search.png
    - assets/icon_more.png
    - assets/icon_bell.png

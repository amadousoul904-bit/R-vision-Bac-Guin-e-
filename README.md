import 'package:flutter/material.dart';

void main() {
  runApp(const RevisionBacGuinee());
}

class RevisionBacGuinee extends StatefulWidget {
  const RevisionBacGuinee({super.key});

  @override
  State<RevisionBacGuinee> createState() => _RevisionBacGuineeState();
}

class _RevisionBacGuineeState extends State<RevisionBacGuinee> {
  ThemeMode themeMode = ThemeMode.light;

  void toggleTheme() {
    setState(() {
      themeMode = themeMode == ThemeMode.light
          ? ThemeMode.dark
          : ThemeMode.light;
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Révision Bac Guinée',
      themeMode: themeMode,

      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF0B8F4D),
        ),
      ),

      darkTheme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF0B8F4D),
          brightness: Brightness.dark,
        ),
      ),

      home: MainScreen(
        darkMode: themeMode == ThemeMode.dark,
        onThemeChanged: toggleTheme,
      ),
    );
  }
}

// ================= LOGO =================

class AppLogo extends StatelessWidget {
  final double size;

  const AppLogo({super.key, this.size = 70});

  @override
  Widget build(BuildContext context) {
    return Container(
      width: size,
      height: size,
      decoration: BoxDecoration(
        gradient: const LinearGradient(
          colors: [
            Color(0xFF0B8F4D),
            Color(0xFF075F34),
          ],
        ),
        borderRadius: BorderRadius.circular(18),
      ),
      child: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(
              Icons.school_rounded,
              color: Colors.white,
              size: size * .38,
            ),
            Text(
              'RBG',
              style: TextStyle(
                color: Colors.white,
                fontSize: size * .20,
                fontWeight: FontWeight.bold,
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// ================= MATIÈRES =================

class Subject {
  final String name;
  final String shortName;
  final IconData icon;
  final Color color;

  const Subject({
    required this.name,
    required this.shortName,
    required this.icon,
    required this.color,
  });
}

const subjects = [
  Subject(
    name: 'Mathématiques',
    shortName: 'Maths',
    icon: Icons.calculate_rounded,
    color: Color(0xFF2878F0),
  ),
  Subject(
    name: 'Français',
    shortName: 'Français',
    icon: Icons.menu_book_rounded,
    color: Color(0xFF8E44AD),
  ),
  Subject(
    name: 'Physique',
    shortName: 'Physique',
    icon: Icons.bolt_rounded,
    color: Color(0xFFE67E22),
  ),
  Subject(
    name: 'Chimie',
    shortName: 'Chimie',
    icon: Icons.science_rounded,
    color: Color(0xFF16A085),
  ),
  Subject(
    name: 'Anglais',
    shortName: 'Anglais',
    icon: Icons.language_rounded,
    color: Color(0xFFE74C3C),
  ),
  Subject(
    name: 'Biologie-Géologie',
    shortName: 'Bio-Géo',
    icon: Icons.eco_rounded,
    color: Color(0xFF27AE60),
  ),
  Subject(
    name: 'Philosophie',
    shortName: 'Philo',
    icon: Icons.psychology_rounded,
    color: Color(0xFF34495E),
  ),
  Subject(
    name: 'Économie',
    shortName: 'Économie',
    icon: Icons.account_balance_rounded,
    color: Color(0xFFF1C40F),
  ),
];

// ================= QUESTIONS =================

class Question {
  final String question;
  final List<String> answers;
  final int correct;

  const Question({
    required this.question,
    required this.answers,
    required this.correct,
  });
}

const questions = [
  Question(
    question: 'Combien font 5 + 7 ?',
    answers: ['10', '11', '12', '13'],
    correct: 2,
  ),
  Question(
    question: 'Quelle est la capitale de la Guinée ?',
    answers: ['Kindia', 'Conakry', 'Labé', 'Kankan'],
    correct: 1,
  ),
  Question(
    question: 'Combien de jours compte une semaine ?',
    answers: ['5', '6', '7', '8'],
    correct: 2,
  ),
  Question(
    question: 'Quelle planète est appelée la planète rouge ?',
    answers: ['Mars', 'Vénus', 'Jupiter', 'Mercure'],
    correct: 0,
  ),
  Question(
    question: 'Quelle est la formule chimique de l’eau ?',
    answers: ['CO2', 'O2', 'H2O', 'NaCl'],
    correct: 2,
  ),
];

// ================= ÉCRAN PRINCIPAL =================

class MainScreen extends StatefulWidget {
  final bool darkMode;
  final VoidCallback onThemeChanged;

  const MainScreen({
    super.key,
    required this.darkMode,
    required this.onThemeChanged,
  });

  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    final pages = [
      HomePage(
        onQuiz: () {
          setState(() => currentIndex = 2);
        },
        onSubjects: () {
          setState(() => currentIndex = 1);
        },
      ),
      const SubjectsPage(),
      const QuizPage(),
      ProfilePage(
        darkMode: widget.darkMode,
        onThemeChanged: widget.onThemeChanged,
      ),
    ];

    return Scaffold(
      body: pages[currentIndex],

      bottomNavigationBar: NavigationBar(
        selectedIndex: currentIndex,
        onDestinationSelected: (index) {
          setState(() => currentIndex = index);
        },
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.home_outlined),
            selectedIcon: Icon(Icons.home),
            label: 'Accueil',
          ),
          NavigationDestination(
            icon: Icon(Icons.menu_book_outlined),
            selectedIcon: Icon(Icons.menu_book),
            label: 'Matières',
          ),
          NavigationDestination(
            icon: Icon(Icons.quiz_outlined),
            selectedIcon: Icon(Icons.quiz),
            label: 'Quiz',
          ),
          NavigationDestination(
            icon: Icon(Icons.person_outline),
            selectedIcon: Icon(Icons.person),
            label: 'Profil',
          ),
        ],
      ),
    );
  }
}

// ================= ACCUEIL =================

class HomePage extends StatelessWidget {
  final VoidCallback onQuiz;
  final VoidCallback onSubjects;

  const HomePage({
    super.key,
    required this.onQuiz,
    required this.onSubjects,
  });

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: SingleChildScrollView(
        padding: const EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                const AppLogo(size: 62),
                const SizedBox(width: 14),

                const Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        'Révision Bac',
                        style: TextStyle(
                          fontSize: 21,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      Text(
                        'GUINÉE 🇬🇳',
                        style: TextStyle(
                          color: Color(0xFF0B8F4D),
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ],
                  ),
                ),

                IconButton(
                  onPressed: () {},
                  icon: const Icon(
                    Icons.notifications_none_rounded,
                  ),
                ),
              ],
            ),

            const SizedBox(height: 28),

            const Text(
              'Bonjour 👋',
              style: TextStyle(
                fontSize: 30,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 6),

            const Text(
              'Prêt à préparer ton Bac aujourd’hui ?',
              style: TextStyle(fontSize: 16),
            ),

            const SizedBox(height: 22),

            // CARTE VERTE
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(22),
              decoration: BoxDecoration(
                gradient: const LinearGradient(
                  colors: [
                    Color(0xFF0B8F4D),
                    Color(0xFF075F34),
                  ],
                ),
                borderRadius: BorderRadius.circular(24),
              ),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Icon(
                    Icons.school_rounded,
                    color: Colors.white,
                    size: 45,
                  ),

                  const SizedBox(height: 15),

                  const Text(
                    'Ton objectif :\nréussir le Bac 🎓',
                    style: TextStyle(
                      color: Colors.white,
                      fontSize: 28,
                      fontWeight: FontWeight.bold,
                    ),
                  ),

                  const SizedBox(height: 12),

                  const Text(
                    'Révise, entraîne-toi et suis ta progression.',
                    style: TextStyle(
                      color: Colors.white70,
                      fontSize: 15,
                    ),
                  ),

                  const SizedBox(height: 20),

                  FilledButton(
                    style: FilledButton.styleFrom(
                      backgroundColor: Colors.white,
                      foregroundColor: const Color(0xFF08783F),
                    ),
                    onPressed: onQuiz,
                    child: const Text(
                      'Commencer un quiz',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                ],
              ),
            ),

            const SizedBox(height: 25),

            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                const Text(
                  'Mes matières',
                  style: TextStyle(
                    fontSize: 21,
                    fontWeight: FontWeight.bold,
                  ),
                ),

                TextButton(
                  onPressed: onSubjects,
                  child: const Text('Voir tout'),
                ),
              ],
            ),

            const SizedBox(height: 12),

            GridView.builder(
              shrinkWrap: true,
              physics: const NeverScrollableScrollPhysics(),
              itemCount: 4,
              gridDelegate:
                  const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 12,
                mainAxisSpacing: 12,
                childAspectRatio: 1.25,
              ),
              itemBuilder: (context, index) {
                return SubjectCard(
                  subject: subjects[index],
                );
              },
            ),

            const SizedBox(height: 25),

            const Text(
              'Ma progression',
              style: TextStyle(
                fontSize: 21,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 12),

            Card(
              child: Padding(
                padding: const EdgeInsets.all(18),
                child: Column(
                  children: [
                    const Row(
                      children: [
                        CircleAvatar(
                          radius: 27,
                          child: Icon(Icons.trending_up),
                        ),
                        SizedBox(width: 14),
                        Expanded(
                          child: Column(
                            crossAxisAlignment:
                                CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Progression générale',
                                style: TextStyle(
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              SizedBox(height: 5),
                              Text('20% terminé'),
                            ],
                          ),
                        ),
                        Text(
                          '20%',
                          style: TextStyle(
                            fontSize: 20,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ],
                    ),

                    const SizedBox(height: 15),

                    const LinearProgressIndicator(
                      value: .20,
                      minHeight: 8,
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// ================= CARTE MATIÈRE =================

class SubjectCard extends StatelessWidget {
  final Subject subject;

  const SubjectCard({
    super.key,
    required this.subject,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: InkWell(
        borderRadius: BorderRadius.circular(16),
        onTap: () {
          Navigator.push(
            context,
            MaterialPageRoute(
              builder: (_) => SubjectDetailPage(
                subject: subject,
              ),
            ),
          );
        },
        child: Padding(
          padding: const EdgeInsets.all(14),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              CircleAvatar(
                backgroundColor:
                    subject.color.withOpacity(.12),
                child: Icon(
                  subject.icon,
                  color: subject.color,
                ),
              ),

              const Spacer(),

              Text(
                subject.shortName,
                style: const TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 3),

              const Text(
                'Cours & quiz',
                style: TextStyle(fontSize: 12),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// ================= MATIÈRES =================

class SubjectsPage extends StatelessWidget {
  const SubjectsPage({super.key});

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: CustomScrollView(
        slivers: [
          const SliverAppBar(
            pinned: true,
            title: Text(
              'Toutes les matières',
              style: TextStyle(
                fontWeight: FontWeight.bold,
              ),
            ),
          ),

          SliverPadding(
            padding: const EdgeInsets.all(16),

            sliver: SliverGrid(
              delegate: SliverChildBuilderDelegate(
                (context, index) {
                  return SubjectCard(
                    subject: subjects[index],
                  );
                },
                childCount: subjects.length,
              ),

              gridDelegate:
                  const SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 2,
                crossAxisSpacing: 12,
                mainAxisSpacing: 12,
                childAspectRatio: 1.15,
              ),
            ),
          ),
        ],
      ),
    );
  }
}

// ================= DÉTAIL MATIÈRE =================

class SubjectDetailPage extends StatelessWidget {
  final Subject subject;

  const SubjectDetailPage({
    super.key,
    required this.subject,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(subject.name),
      ),

      body: ListView(
        padding: const EdgeInsets.all(18),
        children: [
          Container(
            padding: const EdgeInsets.all(22),
            decoration: BoxDecoration(
              color: subject.color.withOpacity(.10),
              borderRadius: BorderRadius.circular(22),
            ),

            child: Row(
              children: [
                CircleAvatar(
                  radius: 30,
                  backgroundColor: subject.color,
                  child: Icon(
                    subject.icon,
                    color: Colors.white,
                    size: 30,
                  ),
                ),

                const SizedBox(width: 15),

                Expanded(
                  child: Text(
                    subject.name,
                    style: const TextStyle(
                      fontSize: 22,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ],
            ),
          ),

          const SizedBox(height: 25),

          CourseTile(
            icon: Icons.play_lesson,
            title: 'Cours 1',
            subtitle: 'Introduction',
          ),

          CourseTile(
            icon: Icons.article,
            title: 'Cours 2',
            subtitle: 'Notions importantes',
          ),

          CourseTile(
            icon: Icons.assignment,
            title: 'Exercices',
            subtitle: 'Entraîne-toi',
          ),

          CourseTile(
            icon: Icons.quiz,
            title: 'Quiz',
            subtitle: 'Teste tes connaissances',
            onTap: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (_) => const QuizPage(),
                ),
              );
            },
          ),
        ],
      ),
    );
  }
}

class CourseTile extends StatelessWidget {
  final IconData icon;
  final String title;
  final String subtitle;
  final VoidCallback? onTap;

  const CourseTile({
    super.key,
    required this.icon,
    required this.title,
    required this.subtitle,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 12),
      child: ListTile(
        leading: CircleAvatar(
          child: Icon(icon),
        ),

        title: Text(
          title,
          style: const TextStyle(
            fontWeight: FontWeight.bold,
          ),
        ),

        subtitle: Text(subtitle),

        trailing: const Icon(
          Icons.arrow_forward_ios,
          size: 17,
        ),

        onTap: onTap ??
            () {
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(
                  content: Text(
                    'Ce contenu sera ajouté prochainement.',
                  ),
                ),
              );
            },
      ),
    );
  }
}

// ================= QUIZ =================

class QuizPage extends StatefulWidget {
  const QuizPage({super.key});

  @override
  State<QuizPage> createState() => _QuizPageState();
}

class _QuizPageState extends State<QuizPage> {
  int questionIndex = 0;
  int score = 0;
  int? selectedAnswer;
  bool answered = false;

  void selectAnswer(int index) {
    if (answered) return;

    setState(() {
      selectedAnswer = index;
      answered = true;

      if (index == questions[questionIndex].correct) {
        score++;
      }
    });

    Future.delayed(
      const Duration(milliseconds: 700),
      () {
        if (!mounted) return;

        if (questionIndex < questions.length - 1) {
          setState(() {
            questionIndex++;
            selectedAnswer = null;
            answered = false;
          });
        } else {
          showResult();
        }
      },
    );
  }

  void showResult() {
    showDialog(
      context: context,
      barrierDismissible: false,
      builder: (context) {
        return AlertDialog(
          icon: const Icon(
            Icons.emoji_events,
            size: 55,
          ),

          title: const Text('Quiz terminé !'),

          content: Text(
            'Tu as obtenu $score/${questions.length}.',
            textAlign: TextAlign.center,
          ),

          actions: [
            TextButton(
              onPressed: () {
                Navigator.pop(context);

                setState(() {
                  questionIndex = 0;
                  score = 0;
                  selectedAnswer = null;
                  answered = false;
                });
              },
              child: const Text('Recommencer'),
            ),

            FilledButton(
              onPressed: () {
                Navigator.pop(context);
                Navigator.pop(context);
              },
              child: const Text('Terminer'),
            ),
          ],
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    final question = questions[questionIndex];

    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Quiz',
          style: TextStyle(
            fontWeight: FontWeight.bold,
          ),
        ),
      ),

      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              children: [
                Text(
                  'Question ${questionIndex + 1}',
                  style: const TextStyle(
                    fontWeight: FontWeight.bold,
                  ),
                ),

                const Spacer(),

                Text(
                  '${questionIndex + 1}/${questions.length}',
                ),
              ],
            ),

            const SizedBox(height: 10),

            LinearProgressIndicator(
              value:
                  (questionIndex + 1) / questions.length,
              minHeight: 8,
            ),

            const SizedBox(height: 35),

            Text(
              question.question,
              style: const TextStyle(
                fontSize: 25,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 30),

            ...List.generate(
              question.answers.length,
              (index) {
                final isCorrect =
                    index == question.correct;

                final isSelected =
                    index == selectedAnswer;

                Color? background;

                if (answered && isCorrect) {
                  background =
                      Colors.green.withOpacity(.15);
                } else if (answered && isSelected) {
                  background =
                      Colors.red.withOpacity(.15);
                }

                return Container(
                  width: double.infinity,
                  margin:
                      const EdgeInsets.only(bottom: 12),

                  child: OutlinedButton(
                    style: OutlinedButton.styleFrom(
                      backgroundColor: background,
                      padding:
                          const EdgeInsets.symmetric(
                        vertical: 17,
                        horizontal: 15,
                      ),
                      alignment: Alignment.centerLeft,
                    ),

                    onPressed: () =>
                        selectAnswer(index),

                    child: Row(
                      children: [
                        CircleAvatar(
                          radius: 16,
                          child: Text(
                            String.fromCharCode(
                              65 + index,
                            ),
                          ),
                        ),

                        const SizedBox(width: 12),

                        Expanded(
                          child: Text(
                            question.answers[index],
                            style: const TextStyle(
                              fontSize: 16,
                              fontWeight: FontWeight.w600,
                            ),
                          ),
                        ),
                      ],
                    ),
                  ),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}

// ================= PROFIL =================

class ProfilePage extends StatelessWidget {
  final bool darkMode;
  final VoidCallback onThemeChanged;

  const ProfilePage({
    super.key,
    required this.darkMode,
    required this.onThemeChanged,
  });

  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: ListView(
        padding: const EdgeInsets.all(20),
        children: [
          const SizedBox(height: 15),

          const Center(
            child: AppLogo(size: 90),
          ),

          const SizedBox(height: 15),

          const Center(
            child: Text(
              'Mon profil',
              style: TextStyle(
                fontSize: 25,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),

          const SizedBox(height: 30),

          Card(
            child: Column(
              children: [
                const ListTile(
                  leading: CircleAvatar(
                    child: Icon(Icons.person),
                  ),
                  title: Text(
                    'Élève',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  subtitle: Text(
                    'Candidat au Bac',
                  ),
                ),

                const Divider(),

                const ListTile(
                  leading: Icon(Icons.star),
                  title: Text('Score'),
                  trailing: Text(
                    '0',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),

                const ListTile(
                  leading: Icon(Icons.menu_book),
                  title: Text('Cours étudiés'),
                  trailing: Text(
                    '0',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ],
            ),
          ),

          const SizedBox(height: 15),

          Card(
            child: SwitchListTile(
              secondary: Icon(
                darkMode
                    ? Icons.dark_mode
                    : Icons.light_mode,
              ),
              title: const Text(
                'Mode sombre',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),
              value: darkMode,
              onChanged: (_) => onThemeChanged(),
            ),
          ),

          const SizedBox(height: 15),

          const Card(
            child: ListTile(
              leading: Icon(Icons.info_outline),
              title: Text(
                'À propos',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),
              subtitle: Text(
                'Révision Bac Guinée\nVersion 1.0.0',
              ),
            ),
          ),
        ],
      ),
    );
  }
}

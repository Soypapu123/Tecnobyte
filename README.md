# Tecnobyte
Pantalla de Bienvenida 
import 'package:flutter/material.dart';
import 'login_screen.dart';

class SplashScreen extends StatefulWidget {
  @override
  _SplashScreenState createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _scaleAnimation;
  late Animation<double> _opacityAnimation;
  late Animation<double> _fadeTextAnimation;
  late Animation<Offset> _slideAnimation;
  bool _isAccepted = false;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: Duration(seconds: 9),
    );

    _scaleAnimation = Tween<double>(begin: 0.3, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.elasticOut),
    );

    _opacityAnimation =
        CurvedAnimation(parent: _controller, curve: Curves.easeInOut);

    _fadeTextAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeIn),
    );

    _slideAnimation =
        Tween<Offset>(begin: Offset(0, 1), end: Offset(0, 0)).animate(
      CurvedAnimation(parent: _controller, curve: Curves.easeOutBack),
    );

    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.deepOrange.shade700, Color(0xF5f5dc)],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Center(
          child: SlideTransition(
            position: _slideAnimation,
            child: FadeTransition(
              opacity: _opacityAnimation,
              child: ScaleTransition(
                scale: _scaleAnimation,
                child: Container(
                  padding: EdgeInsets.symmetric(horizontal: 32, vertical: 24),
                  decoration: BoxDecoration(
                    color: Color(0xF5f5dc).withOpacity(0.9),
                    borderRadius: BorderRadius.circular(20),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.black26,
                        blurRadius: 20,
                        offset: Offset(0, 8),
                      ),
                    ],
                  ),
                  child: Column(
                    mainAxisSize: MainAxisSize.min,
                    children: <Widget>[
                      FadeTransition(
                        opacity: _fadeTextAnimation,
                        child: Image.asset('assets/Tecnobite.png',
                            width: 140, height: 140),
                      ),
                      SizedBox(height: 20),
                      FadeTransition(
                        opacity: _fadeTextAnimation,
                        child: Text(
                          'Bienvenido a TecnoBite',
                          textAlign: TextAlign.center,
                          style: TextStyle(
                            fontSize: 28,
                            color: Color(0xff131212),
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                      SizedBox(height: 20),
                      Row(
                        children: [
                          Checkbox(
                            value: _isAccepted,
                            onChanged: (value) {
                              setState(() {
                                _isAccepted = value ?? false;
                              });
                            },
                          ),
                          Expanded(
                            child: Text(
                              'Acepto los términos y condiciones',
                              style: TextStyle(
                                  fontSize: 14, color: Color(0xff050505)),
                            ),
                          ),
                        ],
                      ),
                      SizedBox(height: 20),
                      FadeTransition(
                        opacity: _fadeTextAnimation,
                        child: ElevatedButton.icon(
                          onPressed: _isAccepted
                              ? () {
                                  Navigator.pushReplacement(
                                    context,
                                    MaterialPageRoute(
                                        builder: (context) => LoginScreen()),
                                  );
                                }
                              : null,
                          icon: Icon(Icons.login, color: Colors.white),
                          label: Text(
                            'Iniciar',
                            style: TextStyle(
                                fontSize: 18, fontWeight: FontWeight.bold),
                          ),
                          style: ElevatedButton.styleFrom(
                            backgroundColor: Color(0xffdb4416),
                            padding: EdgeInsets.symmetric(
                                horizontal: 50, vertical: 18),
                            shape: RoundedRectangleBorder(
                              borderRadius: BorderRadius.circular(30),
                            ),
                            elevation: 10,
                            disabledBackgroundColor: Color(0xffeeeded),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}


![image](https://github.com/user-attachments/assets/5bdef3ea-3696-4154-9938-0fee5cf5f415)
![image](https://github.com/user-attachments/assets/c0893d85-1050-418d-9d32-2422d11cfac2)



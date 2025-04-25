# Tecnobyte
Pantalla de Bienvenida:

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

Pantalla de registro
import 'package:flutter/material.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'login_screen.dart';
import 'home_screen.dart';

class RegisterScreenPage extends StatefulWidget {
  @override
  _RegisterScreenPageState createState() => _RegisterScreenPageState();
}

class _RegisterScreenPageState extends State<RegisterScreenPage> {
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();
  final TextEditingController _confirmPasswordController =
      TextEditingController();
  final FirebaseAuth _auth = FirebaseAuth.instance;
  bool _obscurePassword = true;

  final RegExp passwordRegex = RegExp(r'^[a-zA-Z0-9!@#\$%\^&\*\(\)_\-+=]{7,}$');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xFFF5F5DC), // Beige
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(24.0),
          child: Card(
            elevation: 10,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
            color: Color(0xFFDB4416), // Naranja oscuro
            child: Padding(
              padding: const EdgeInsets.all(24.0),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: <Widget>[
                  Text(
                    'Registrarse',
                    style: TextStyle(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                    ),
                  ),
                  SizedBox(height: 20),
                  TextField(
                    controller: _emailController,
                    decoration: InputDecoration(
                      labelText: 'Email',
                      labelStyle: TextStyle(color: Colors.white),
                      focusedBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white),
                      ),
                      enabledBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white70),
                      ),
                      border: OutlineInputBorder(),
                    ),
                  ),
                  SizedBox(height: 16),
                  TextField(
                    controller: _passwordController,
                    obscureText: _obscurePassword,
                    decoration: InputDecoration(
                      labelText: 'Contraseña',
                      labelStyle: TextStyle(color: Colors.white),
                      focusedBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white),
                      ),
                      enabledBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white70),
                      ),
                      border: OutlineInputBorder(),
                      suffixIcon: IconButton(
                        icon: Icon(
                          _obscurePassword
                              ? Icons.visibility
                              : Icons.visibility_off,
                          color: Colors.white,
                        ),
                        onPressed: () {
                          setState(() {
                            _obscurePassword = !_obscurePassword;
                          });
                        },
                      ),
                    ),
                  ),
                  SizedBox(height: 16),
                  TextField(
                    controller: _confirmPasswordController,
                    obscureText: _obscurePassword,
                    decoration: InputDecoration(
                      labelText: 'Confirmar Contraseña',
                      labelStyle: TextStyle(color: Colors.white),
                      focusedBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white),
                      ),
                      enabledBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white70),
                      ),
                      border: OutlineInputBorder(),
                    ),
                  ),
                  SizedBox(height: 24),
                  ElevatedButton(
                    onPressed: () async {
                      String password = _passwordController.text;
                      String confirmPassword = _confirmPasswordController.text;

                      if (password != confirmPassword) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(
                              content: Text('Las contraseñas no coinciden.')),
                        );
                        return;
                      }

                      if (!passwordRegex.hasMatch(password)) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(
                              content: Text(
                                  'Contraseña inválida. Usa al menos 7 caracteres válidos sin símbolos prohibidos.')),
                        );
                        return;
                      }

                      try {
                        UserCredential userCredential =
                            await _auth.createUserWithEmailAndPassword(
                          email: _emailController.text,
                          password: password,
                        );
                        Navigator.pushReplacement(
                          context,
                          MaterialPageRoute(builder: (context) => HomeScreen()),
                        );
                      } catch (e) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(content: Text('Error: ${e.toString()}')),
                        );
                      }
                    },
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Color(0xF5f5dc),
                      foregroundColor: Colors.white,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      padding:
                          EdgeInsets.symmetric(vertical: 12, horizontal: 32),
                    ),
                    child: Text('Registrarse', style: TextStyle(fontSize: 16)),
                  ),
                  SizedBox(height: 16),
                  TextButton(
                    onPressed: () {
                      Navigator.pushReplacement(
                        context,
                        MaterialPageRoute(builder: (context) => LoginScreen()),
                      );
                    },
                    child: Text(
                      '¿Ya tienes cuenta? Inicia sesión',
                      style: TextStyle(color: Color(0xff000000)),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),

    );
  }
}
![image](https://github.com/user-attachments/assets/c2fbe595-76d9-46e4-b7ab-a42a65ccec8c)

Pantalla de recuperacion de contraseña

import 'package:flutter/material.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'login_screen.dart';

class ForgotPasswordScreen extends StatelessWidget {
  final TextEditingController _emailController = TextEditingController();
  final FirebaseAuth _auth = FirebaseAuth.instance;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Color(0xfff5f5dc), // Beige claro
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(24.0),
          child: Card(
            elevation: 10,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(16),
            ),
            color: Color(0xffdb4416), // Rojo anaranjado
            child: Padding(
              padding: const EdgeInsets.all(24.0),
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: <Widget>[
                  Text(
                    'Recuperar Contraseña',
                    style: TextStyle(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                    ),
                  ),
                  SizedBox(height: 20),
                  TextField(
                    controller: _emailController,
                    decoration: InputDecoration(
                      labelText: 'Email',
                      labelStyle: TextStyle(color: Colors.black),
                      focusedBorder: OutlineInputBorder(
                        borderSide: BorderSide(color: Colors.white),
                      ),
                      border: OutlineInputBorder(),
                      fillColor: Colors.white,
                      filled: true,
                    ),
                  ),
                  SizedBox(height: 24),
                  ElevatedButton(
                    onPressed: () async {
                      try {
                        await _auth.sendPasswordResetEmail(
                            email: _emailController.text);
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(content: Text('Se ha enviado el enlace')),
                        );
                        Navigator.pushReplacement(
                          context,
                          MaterialPageRoute(
                              builder: (context) => LoginScreen()),
                        );
                      } catch (e) {
                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(content: Text('Error: {e.toString()}')),
                        );
                      }
                    },
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Color(0xF5f5dc), // Rojo anaranjado
                      foregroundColor: Colors.white,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      padding:
                          EdgeInsets.symmetric(vertical: 12, horizontal: 32),
                    ),
                    child: Text('Enviar enlace para restablecer',
                        style: TextStyle(fontSize: 16)),
                  ),
                  SizedBox(height: 16),
                  TextButton(
                    onPressed: () {
                      Navigator.pushReplacement(
                        context,
                        MaterialPageRoute(builder: (context) => LoginScreen()),
                      );
                    },
                    child: Text(
                      'Volver al inicio de sesión',
                      style: TextStyle(color: Color(0xff131212)),
                    ),
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }
}
![image](https://github.com/user-attachments/assets/f763522f-9a76-4dd0-a211-267c38b644b7)

Pantalla de menu

import 'package:flutter/material.dart';
import 'glosario_screen.dart';
import 'calculadora_resistencias.dart';
import 'login_screen.dart';
import 'microcontroladores_screen.dart';
import 'Formulas.dart';
import 'Guia_de_usuario.dart';
import 'calculadora_cientifica.dart'; // Verifica la ruta correcta de este archivo

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Row(
          children: [
            Icon(Icons.memory, color: Colors.white),
            SizedBox(width: 10),
            Text('TecnoByte'),
          ],
        ),
        backgroundColor: Color(0xffdb4416), // Naranja
        actions: [
          IconButton(
            icon: Icon(Icons.logout, color: Colors.white),
            onPressed: () {
              _showLogoutConfirmation(context);
            },
          ),
        ],
      ),
      backgroundColor: Color(0xFFF5F5DC), // Beige
      body: ListView(
        padding: EdgeInsets.all(16.0),
        children: [
          Card(
            color: Color(0xFFFFEFD5), // PapayaWhip claro
            elevation: 4,
            shape:
                RoundedRectangleBorder(borderRadius: BorderRadius.circular(16)),
            child: Padding(
              padding: const EdgeInsets.all(20.0),
              child: Column(
                children: [
                  Icon(Icons.electrical_services,
                      size: 60, color: Color(0xFF1E3A8A)),
                  SizedBox(height: 10),
                  Text(
                    'Bienvenido a TecnoByte',
                    style: TextStyle(
                      fontSize: 22,
                      fontWeight: FontWeight.bold,
                      color: Color(0xFF1E3A8A),
                    ),
                    textAlign: TextAlign.center,
                  ),
                  SizedBox(height: 15),
                  Text(
                    'TecnoByte es una aplicación de electrónica diseñada para los estudiantes de bachillerato del CBTIS 128. '
                    'Es una herramienta esencial para su aprendizaje, proporcionando recursos interactivos y útiles para facilitar '
                    'el estudio de la electrónica. Además, es una herramienta valiosa para los docentes que imparten clases en esta área, '
                    'ayudándolos a mejorar la enseñanza con contenido didáctico y práctico. '
                    'Aplicación de electrónica para estudiantes del CBTIS 128. Ideal para aprender, repasar y aplicar conocimientos '
                    'con recursos interactivos.',
                    textAlign: TextAlign.center,
                    style: TextStyle(fontSize: 16, color: Colors.black87),
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
      drawer: Drawer(
        child: Container(
          color: Color(0xFFF5F5DC),
          child: ListView(
            padding: EdgeInsets.zero,
            children: [
              DrawerHeader(
                decoration: BoxDecoration(
                  color: Color(0xFFFFA500),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Icon(Icons.settings_suggest, size: 40, color: Colors.white),
                    SizedBox(height: 10),
                    Text(
                      'Menú Principal',
                      style: TextStyle(
                        color: Colors.white,
                        fontSize: 22,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ],
                ),
              ),
              CustomMenuItem(
                title: 'Calculadora de Resistencias',
                screen: CalculadoraResistenciasScreen(),
                icon: Icons.calculate,
              ),
              CustomMenuItem(
                title: 'Glosario de Electrónica',
                screen: GlosarioScreen(),
                icon: Icons.book,
              ),
              CustomMenuItem(
                title: 'Microcontroladores',
                screen: MicrocontroladoresScreen(),
                icon: Icons.computer,
              ),
              CustomMenuItem(
                title: 'Fórmulas',
                screen: FormulasScreen(),
                icon: Icons.functions,
              ),
              CustomMenuItem(
                title: 'Guía de Usuario',
                screen: GuiaDeUsuarioScreen(),
                icon: Icons.help_outline,
              ),
              CustomMenuItem(
                title: 'Calculadora Científica',
                screen:
                    CalculadoraCientificaScreen(), // Verifica que esta importación sea correcta
                icon: Icons.science,
              ),
              Divider(),
            ],
          ),
        ),
      ),
    );
  }

  void _showLogoutConfirmation(BuildContext context) {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text('Cerrar Sesión'),
          content: Text('¿Estás seguro de que deseas cerrar sesión?'),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.of(context).pop(); // Cierra el diálogo
              },
              child: Text('Cancelar'),
            ),
            TextButton(
              onPressed: () {
                Navigator.of(context).pop(); // Cierra el diálogo
                Navigator.pushReplacement(
                  context,
                  MaterialPageRoute(builder: (context) => LoginScreen()),
                );
              },
              child: Text('Cerrar Sesión'),
            ),
          ],
        );
      },
    );
  }
}

class CustomMenuItem extends StatelessWidget {
  final String title;
  final Widget screen;
  final IconData icon;

  CustomMenuItem({
    required this.title,
    required this.screen,
    required this.icon,
  });

  @override
  Widget build(BuildContext context) {
    return ListTile(
      leading: Icon(icon, color: Color(0xFFFFA500)), // Naranja
      title: Text(
        title,
        style: TextStyle(
          fontWeight: FontWeight.bold,
          color: Color(0xFF1E3A8A), // Azul Marino
        ),
      ),
      onTap: () {
        Navigator.pop(context); // Cierra el Drawer
        Navigator.push(
          context,
          MaterialPageRoute(builder: (context) => screen),
        );
      },
    );
  }
}

![image](https://github.com/user-attachments/assets/a7e64cf4-d1f5-4e2c-84ff-f05f8fb02879)
![image](https://github.com/user-attachments/assets/a892df19-1a64-4c52-a9b2-13515ccac7e8)

Pantalla de caculadora de resistencias

import 'package:flutter/material.dart';

class CalculadoraResistenciasScreen extends StatefulWidget {
  @override
  _CalculadoraResistenciasScreenState createState() =>
      _CalculadoraResistenciasScreenState();
}

class _CalculadoraResistenciasScreenState
    extends State<CalculadoraResistenciasScreen> {
  final List<String> colores = [
    'Negro',
    'Marrón',
    'Rojo',
    'Naranja',
    'Amarillo',
    'Verde',
    'Azul',
    'Violeta',
    'Gris',
    'Blanco'
  ];

  String? banda1;
  String? banda2;
  String? banda3;
  String? multiplicador;
  String unidad = 'Ω'; // Valor por defecto en ohmios

  String resultado = '';

  void calcularResistencia() {
    if (banda1 != null &&
        banda2 != null &&
        banda3 != null &&
        multiplicador != null) {
      int valor = (getColorValue(banda1!) * 10 + getColorValue(banda2!)) *
          getColorMultiplier(multiplicador!);

      // Convertir el valor según la unidad seleccionada
      double resistenciaConvertida;
      switch (unidad) {
        case 'kΩ':
          resistenciaConvertida = valor / 1000.0;
          break;
        case 'MΩ':
          resistenciaConvertida = valor / 1000000.0;
          break;
        case 'GΩ':
          resistenciaConvertida = valor / 1000000000.0;
          break;
        default:
          resistenciaConvertida = valor.toDouble();
          break;
      }

      setState(() {
        resultado =
            'Valor de la resistencia: ${resistenciaConvertida.toStringAsFixed(2)} $unidad';
      });
    }
  }

  int getColorValue(String color) {
    switch (color) {
      case 'Negro':
        return 0;
      case 'Marrón':
        return 1;
      case 'Rojo':
        return 2;
      case 'Naranja':
        return 3;
      case 'Amarillo':
        return 4;
      case 'Verde':
        return 5;
      case 'Azul':
        return 6;
      case 'Violeta':
        return 7;
      case 'Gris':
        return 8;
      case 'Blanco':
        return 9;
      default:
        return 0;
    }
  }

  int getColorMultiplier(String color) {
    switch (color) {
      case 'Negro':
        return 1;
      case 'Marrón':
        return 10;
      case 'Rojo':
        return 100;
      case 'Naranja':
        return 1000;
      case 'Amarillo':
        return 10000;
      case 'Verde':
        return 100000;
      case 'Azul':
        return 1000000;
      case 'Violeta':
        return 10000000;
      case 'Gris':
        return 100000000;
      case 'Blanco':
        return 1000000000;
      default:
        return 1;
    }
  }

  Color getColor(String color) {
    switch (color) {
      case 'Negro':
        return Colors.black;
      case 'Marrón':
        return Colors.brown;
      case 'Rojo':
        return Colors.red;
      case 'Naranja':
        return Colors.orange;
      case 'Amarillo':
        return Colors.yellow;
      case 'Verde':
        return Colors.green;
      case 'Azul':
        return Colors.blue;
      case 'Violeta':
        return Colors.purple;
      case 'Gris':
        return Colors.grey;
      case 'Blanco':
        return Colors.white;
      default:
        return Colors.transparent;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Calculadora de Resistencias'),
        backgroundColor: Colors.orange, // Cambiar el color de la AppBar
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Selecciona los colores de las bandas:',
                style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.bold,
                    color: Colors.black)),
            SizedBox(height: 16),
            _buildColorSelector('Banda 1', banda1, (value) {
              setState(() {
                banda1 = value;
              });
            }),
            _buildColorSelector('Banda 2', banda2, (value) {
              setState(() {
                banda2 = value;
              });
            }),
            _buildColorSelector('Banda 3', banda3, (value) {
              setState(() {
                banda3 = value;
              });
            }),
            _buildColorSelector('Multiplicador', multiplicador, (value) {
              setState(() {
                multiplicador = value;
              });
            }),
            SizedBox(height: 20),
            _buildUnidadSelector(),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: calcularResistencia,
              child: Text('Calcular Resistencia'),
              style: ElevatedButton.styleFrom(backgroundColor: Colors.orange),
            ),
            SizedBox(height: 20),
            if (resultado.isNotEmpty)
              Text(
                resultado,
                style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
              ),
            SizedBox(height: 20),
            if (banda1 != null && banda2 != null && banda3 != null)
              _buildResistorBandsPreview(),
          ],
        ),
      ),
    );
  }

  Widget _buildColorSelector(
      String label, String? value, Function(String?) onChanged) {
    return DropdownButton<String>(
      isExpanded: true,
      hint: Text('Seleccionar $label'),
      value: value,
      onChanged: (newValue) => onChanged(newValue),
      items: colores.map((color) {
        return DropdownMenuItem(
          value: color,
          child: Text(color),
        );
      }).toList(),
    );
  }

  Widget _buildUnidadSelector() {
    return DropdownButton<String>(
      value: unidad,
      isExpanded: true,
      onChanged: (value) {
        setState(() {
          unidad = value!;
        });
      },
      items: [
        DropdownMenuItem(value: 'Ω', child: Text('Ohmios (Ω)')),
        DropdownMenuItem(value: 'kΩ', child: Text('Kiloohmios (kΩ)')),
        DropdownMenuItem(value: 'MΩ', child: Text('Megaohmios (MΩ)')),
        DropdownMenuItem(value: 'GΩ', child: Text('Gigaohmios (GΩ)')),
      ],
    );
  }

  Widget _buildResistorBandsPreview() {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        _buildBandPreview(banda1),
        _buildBandPreview(banda2),
        _buildBandPreview(banda3),
        _buildBandPreview(multiplicador),
      ],
    );
  }

  Widget _buildBandPreview(String? color) {
    return Container(
      width: 30,
      height: 80,
      color: color != null ? getColor(color) : Colors.transparent,
    );
  }
}
![image](https://github.com/user-attachments/assets/e4f7213e-5a1f-4f33-904a-8999bdf7a5b5)

Pantalla de glosario de electronica






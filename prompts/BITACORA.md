# Bitacora de prompts
Laboratorio 06: Fundamentos de Ingenieria de Prompts.
Herramienta de IA usada: (escribe aqui cual usaste)
## Ejercicio 2: Tokens y ventana de contexto
| Texto | Caracteres | Tokens |
|-------|------------|--------|
| Los estudiantes programan en Java. |7 | 35|
| The students program in Java. |6 |30 |
| desafortunadamente |5|19 |
Lo que sucedio en los pasos 4 y 5 en mi caso fue que mi IA que es CLAUDE tiene una funcion de memoria por lo cual respondio mi pregunta por mas que no le di el contexto.
## Ejercicio 3: Temperatura
| Temperatura | % de BiblioTec | Nombres en los 5 intentos |
|-------------|----------------|---------------------------|
| 0 |100% |BiblioTec, BiblioTec, BiblioTec, BiblioTec, BiblioTec |
| 0.5 |65.3% |PrestaLibro, BiblioTec, BiblioTec, BiblioTec, LibroYa |
| 1  |44.5% |PrestaLibro, BiblioTec, BiblioTec, BiblioTec, LibroYa |
| 1.8 |32.2% |BiblioTec, LibroYa, BiblioTec, LibroYa, BiblioTec |
Lo que pasa es que al subir la temperatura esta reparte un porcentaje a un libro
## Ejercicio 4: Prompt vago vs estructurado
| Criterio | Prompt vago | Prompt estructurado |
|----------|-------------|---------------------|
| Menciona el objetivo del sistema |si |si |
| Menciona a los usuarios principales |si |si |
| Tiene exactamente 3 funcionalidades |no |si |
| Esta en 3 parrafos |no |si |
| Lo usaria en un informe real |no |si |
## Ejercicio 5: Anatomia de un prompt
| Componente | Texto de mi prompt |
|------------|--------------------|
| Rol |desarrollador Java |
| Instruccion |usando una clase Producto |
| Contexto |No especifica claramente |
| Ejemplo |Usa este estilo para los métodos: getPrecio(), setPrecio(double precio). |
| Formato |No especifica claramente |
Cambio mucho a comparacion de los demas ya que le dimos algo mas por hacer y como hacer o que agregar
## Ejercicio 6: Del prompt basico al profesional
| Qué revisar| Cumple (SI/NO)     |
|------------|--------------------|
| ¿Pide correo y contraseña? |SI |
| ¿Explica el funcionamiento antes o después del código? |SI |
| ¿El código está organizado en clases? |SI |
| ¿Valida los datos que ingresa el usuario? |SI  |
```text
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.util.HashMap;
import java.util.Map;

// ===========================
// Clase modelo: Usuario
// ===========================
class Usuario {
    private String correo;
    private String contrasena;

    public Usuario(String correo, String contrasena) {
        this.correo = correo;
        this.contrasena = contrasena;
    }

    public String getCorreo() {
        return correo;
    }

    public String getContrasena() {
        return contrasena;
    }
}

// ===========================
// Clase de lógica: AutenticadorService
// ===========================
class AutenticadorService {
    private Map<String, Usuario> usuariosRegistrados;

    public AutenticadorService() {
        usuariosRegistrados = new HashMap<>();
        // Usuarios de prueba (en un caso real vendrían de una BD)
        usuariosRegistrados.put("admin@tiendatec.com", new Usuario("admin@tiendatec.com", "clave1234"));
        usuariosRegistrados.put("jordan@tiendatec.com", new Usuario("jordan@tiendatec.com", "abcdefgh"));
    }

    public boolean validarCredenciales(String correo, String contrasena) {
        Usuario usuario = usuariosRegistrados.get(correo);
        if (usuario == null) {
            return false;
        }
        return usuario.getContrasena().equals(contrasena);
    }
}

// ===========================
// Clase de validación: ValidadorFormulario
// ===========================
class ValidadorFormulario {

    public static boolean correoValido(String correo) {
        return correo.contains("@");
    }

    public static boolean contrasenaValida(String contrasena) {
        return contrasena.length() >= 8;
    }
}

// ===========================
// Clase vista: LoginFrame
// ===========================
class LoginFrame extends JFrame {
    private JTextField campoCorreo;
    private JPasswordField campoContrasena;
    private JButton botonIngresar;
    private AutenticadorService autenticadorService;

    public LoginFrame() {
        autenticadorService = new AutenticadorService();
        configurarVentana();
        inicializarComponentes();
    }

    private void configurarVentana() {
        setTitle("TiendaTec - Iniciar sesión");
        setSize(350, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setResizable(false);
    }

    private void inicializarComponentes() {
        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(8, 8, 8, 8);
        gbc.fill = GridBagConstraints.HORIZONTAL;

        // Etiqueta y campo de correo
        gbc.gridx = 0;
        gbc.gridy = 0;
        panel.add(new JLabel("Correo:"), gbc);

        campoCorreo = new JTextField(18);
        gbc.gridx = 1;
        panel.add(campoCorreo, gbc);

        // Etiqueta y campo de contraseña
        gbc.gridx = 0;
        gbc.gridy = 1;
        panel.add(new JLabel("Contraseña:"), gbc);

        campoContrasena = new JPasswordField(18);
        gbc.gridx = 1;
        panel.add(campoContrasena, gbc);

        // Botón de ingresar
        botonIngresar = new JButton("Ingresar");
        gbc.gridx = 0;
        gbc.gridy = 2;
        gbc.gridwidth = 2;
        panel.add(botonIngresar, gbc);

        // Evento del botón
        botonIngresar.addActionListener(this::onIngresarClick);

        add(panel);
    }

    private void onIngresarClick(ActionEvent evento) {
        String correo = campoCorreo.getText().trim();
        String contrasena = new String(campoContrasena.getPassword());

        // Validación: campos vacíos
        if (correo.isEmpty() || contrasena.isEmpty()) {
            JOptionPane.showMessageDialog(this,
                    "Debe ingresar correo y contraseña",
                    "Campos incompletos",
                    JOptionPane.WARNING_MESSAGE);
            return;
        }

        // Validación: formato de correo
        if (!ValidadorFormulario.correoValido(correo)) {
            JOptionPane.showMessageDialog(this,
                    "El correo ingresado no es válido (debe contener @)",
                    "Correo inválido",
                    JOptionPane.WARNING_MESSAGE);
            return;
        }

        // Validación: longitud de contraseña
        if (!ValidadorFormulario.contrasenaValida(contrasena)) {
            JOptionPane.showMessageDialog(this,
                    "La contraseña debe tener al menos 8 caracteres",
                    "Contraseña inválida",
                    JOptionPane.WARNING_MESSAGE);
            return;
        }

        // Validación: credenciales contra el servicio
        boolean credencialesValidas = autenticadorService.validarCredenciales(correo, contrasena);

        if (credencialesValidas) {
            JOptionPane.showMessageDialog(this,
                    "Bienvenido, " + correo,
                    "Acceso concedido",
                    JOptionPane.INFORMATION_MESSAGE);
            // Aquí podrías abrir la ventana principal de TiendaTec
        } else {
            JOptionPane.showMessageDialog(this,
                    "Correo o contraseña incorrectos",
                    "Error de autenticación",
                    JOptionPane.ERROR_MESSAGE);
            campoContrasena.setText("");
        }
    }
}

// ===========================
// Clase principal: Main
// ===========================
public class Main {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            LoginFrame login = new LoginFrame();
            login.setVisible(true);
        });
    }
}
```

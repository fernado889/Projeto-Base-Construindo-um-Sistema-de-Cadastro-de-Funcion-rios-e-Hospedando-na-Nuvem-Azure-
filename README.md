# Projeto-Base-Construindo-um-Sistema-de-Cadastro-de-Funcion-rios-e-Hospedando-na-Nuvem-Azure-
public class Funcionario {

  private int id;

  private String nome;

  private String cargo;

  private double salario;



  public Funcionario(int id, String nome, String cargo, double salario) {

    this.id = id;

    this.nome = nome;

    this.cargo = cargo;

    this.salario = salario;

  }



  public int getId() {

    return id;

  }



  public void setId(int id) {

    this.id = id;

  }



  public String getNome() {

    return nome;

  }



  public void setNome(String nome) {

    this.nome = nome;

  }



  public String getCargo() {

    return cargo;

  }



  public void setCargo(String cargo) {

    this.cargo = cargo;

  }



  public double getSalario() {

    return salario;

  }



  public void setSalario(double salario) {

    this.salario = salario;

  }

}

import java.sql.Connection;

import java.sql.DriverManager;

import java.sql.PreparedStatement;

import java.sql.ResultSet;

import java.sql.SQLException;

import java.util.ArrayList;

import java.util.List;



public class FuncionarioDAO {

  private Connection conexao;



  public FuncionarioDAO() {

    try {

      conexao = DriverManager.getConnection("jdbc:mysql://localhost:3306/funcionarios", "usuario", "senha");

    } catch (SQLException e) {

      System.out.println("Erro ao conectar ao banco de dados: " + e.getMessage());

    }

  }



  public void cadastrarFuncionario(Funcionario funcionario) {

    try {

      PreparedStatement stmt = conexao.prepareStatement("INSERT INTO funcionarios (nome, cargo, salario) VALUES (?, ?, ?)");

      stmt.setString(1, funcionario.getNome());

      stmt.setString(2, funcionario.getCargo());

      stmt.setDouble(3, funcionario.getSalario());

      stmt.executeUpdate();

    } catch (SQLException e) {

      System.out.println("Erro ao cadastrar funcionário: " + e.getMessage());

    }

  }



  public List<Funcionario> listarFuncionarios() {

    List<Funcionario> funcionarios = new ArrayList<>();

    try {

      PreparedStatement stmt = conexao.prepareStatement("SELECT * FROM funcionarios");

      ResultSet rs = stmt.executeQuery();

      while (rs.next()) {

        Funcionario funcionario = new Funcionario(rs.getInt("id"), rs.getString("nome"), rs.getString("cargo"), rs.getDouble("salario"));

        funcionarios.add(funcionario);

      }

    } catch (SQLException e) {

      System.out.println("Erro ao listar funcionários: " + e.getMessage());

    }

    return funcionarios;

  }

}

import java.util.List;



public class Main {

  public static void main(String[] args) {

    FuncionarioDAO dao = new FuncionarioDAO();



    // Cadastrar funcionário

    Funcionario funcionario = new Funcionario(1, "João", "Desenvolvedor", 5000.0);

    dao.cadastrarFuncionario(funcionario);



    // Listar funcionários

    List<Funcionario> funcionarios = dao.listarFuncionarios();

    for (Funcionario f : funcionarios) {

      System.out.println("ID: " + f.getId() + ", Nome: " + f.getNome() + ", Cargo: " + f.getCargo() + ", Salário: " + f.getSalario());

    }

  }

}

az mysql server create --resource-group meu-grupo-de-recursos --name meu-banco-de-dados --location eastus --admin-user meu-usuario --admin-password minha-senha

az webapp create --resource-group meu-grupo-de-recursos --name meu-aplicativo-web --location eastus --plan meu-plano-de-servico

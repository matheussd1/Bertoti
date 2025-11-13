# Bertoti
## 1.
O texto reflete sobre a diferença entre programar e fazer engenharia de software. Programar é escrever código, enquanto engenharia de software envolve aplicar teoria e boas práticas para criar algo confiável e duradouro, assim como ocorre em outras áreas da engenharia. Apesar disso, historicamente a área de software foi menos rigorosa, mas conforme o software se torna parte essencial da vida, cresce a necessidade de métodos mais cuidadosos e estruturados.

## 2.
O foco aqui é ampliar o conceito de engenharia de software, mostrando que ele inclui todo o conjunto de ferramentas e processos usados para manter o código ao longo do tempo. É preciso pensar em como o código e a organização vão evoluir e como tomar decisões equilibrando custos e benefícios. A ideia central é que engenharia de software é programação somada ao tempo, ou seja, projetar desde o início para que o código seja sustentável e adaptável.

## 3. Trade-Offs
1. Velocidade de entrega vs. qualidade a longo prazo.
Entregar rapidamente atende demandas imediatas, mas pode gerar dívidas técnicas e dificultar a manutenção no futuro.
2. Rigor técnico vs. flexibilidade inicial.
Seguir padrões e práticas rigorosas desde o início aumenta a confiabilidade, mas pode reduzir a agilidade no começo do projeto.
3. Escalabilidade organizacional vs. simplicidade de processos. 

Criar processos para lidar com crescimento ajuda na organização e manutenção do código, mas pode adicionar burocracia e dificultar mudanças rápidas.

## 4. Diagrama de Classes UML
<img width="970" height="542" alt="image" src="https://github.com/user-attachments/assets/1cf773f0-46cc-4e90-93c4-57465287fe68" />

## 5. Java respectivo
### Pessoa

//Pessoa.java

package hotel;

public abstract class Pessoa {

    protected String nome;
    protected int numCel;
    protected int rg;
    
    public Pessoa(String nome, int numCel, int rg) {
        this.nome = nome;
        this.numCel = numCel;
        this.rg = rg;
    }

    // Getters e Setters
    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public int getNumCel() {
        return numCel;
    }

    public void setNumCel(int numCel) {
        this.numCel = numCel;
    }

    public int getRg() {
        return rg;
    }

    public void setRg(int rg) {
        this.rg = rg;
    }
}

### Cliente

//Cliente.java

package hotel;

import java.util.ArrayList;
import java.util.List;

public class Cliente extends Pessoa {

    private List<Quarto> quartos;

    //1 Cliente pode ter 0 ou mais Contas (0..*)
    private List<Conta> contas;

    public Cliente(String nome, int numCel, int rg) {
        super(nome, numCel, rg);
        this.quartos = new ArrayList<>();
        this.contas = new ArrayList<>();
    }

    public void checkIn(Quarto quarto) {
        System.out.println("Cliente " + this.nome + " fazendo check-in no quarto " + quarto.getNumQuarto());
        quarto.setCliente(this); // Define este cliente como o ocupante do quarto
        this.quartos.add(quarto); // Adiciona o quarto à lista do cliente
    }

    public void checkOut(Quarto quarto) {
        System.out.println("Cliente " + this.nome + " fazendo check-out do quarto " + quarto.getNumQuarto());
        quarto.setCliente(null); // Libera o quarto
    }

    public void pagarConta(Conta conta) {
        System.out.println("Cliente " + this.nome + " pagando a conta " + conta.getNumConta());
    }

    // Getters e Setters
    public List<Quarto> getQuartos() {
        return quartos;
    }

    public List<Conta> getContas() {
        return contas;
    }

    public void addConta(Conta conta) {
        this.contas.add(conta);
    }
}

### Recepcionista

//Recepcionista.java

package hotel;

import java.util.ArrayList;
import java.util.List;

public class Recepcionista extends Pessoa {

    private List<Quarto> quartosGerenciados;

    public Recepcionista(String nome, int numCel, int rg) {
        super(nome, numCel, rg);
        this.quartosGerenciados = new ArrayList<>();
    }

    public void verificarQuarto(Quarto quarto) {
        System.out.println("Verificando quarto: " + quarto.getNumQuarto());
    }

    public void agendar(Cliente cliente, Quarto quarto) {
        System.out.println("Agendando quarto " + quarto.getNumQuarto() + " para " + cliente.getNome());
    }

    public Conta gerarConta(Cliente cliente) {
        System.out.println("Gerando conta para: " + cliente.getNome());
        Conta novaConta = new Conta(12345, cliente); 
        return novaConta;
    }

    // Getters e Setters
    public List<Quarto> getQuartosGerenciados() {
        return quartosGerenciados;
    }

    public void addQuartoGerenciado(Quarto quarto) {
        this.quartosGerenciados.add(quarto);
        quarto.setRecepcionista(this);
    }
}

### quarto

// Quarto.java

package hotel;

public class Quarto {

    private int numQuarto;
    private String localizacao;

    private Recepcionista recepcionista;

    // 1 Quarto pode estar associado a 0 ou 1 Cliente (0..1)
    private Cliente cliente; // null se estiver vago

    public Quarto(int numQuarto, String localizacao, Recepcionista recepcionista) {
        this.numQuarto = numQuarto;
        this.localizacao = localizacao;
        this.recepcionista = recepcionista;
        this.cliente = null; // Quarto começa vago
    }

    // Getters e Setters
    public int getNumQuarto() {
        return numQuarto;
    }

    public void setNumQuarto(int numQuarto) {
        this.numQuarto = numQuarto;
    }

    public String getLocalizacao() {
        return localizacao;
    }

    public void setLocalizacao(String localizacao) {
        this.localizacao = localizacao;
    }

    public Recepcionista getRecepcionista() {
        return recepcionista;
    }

    public void setRecepcionista(Recepcionista recepcionista) {
        this.recepcionista = recepcionista;
    }

    public Cliente getCliente() {
        return cliente;
    }

    public void setCliente(Cliente cliente) {
        this.cliente = cliente;
    }
}

### Conta

// Conta.java

package hotel;

public class Conta {

    private int numConta;

    // 1 Conta pertence a exatamente 1 Cliente
    private Cliente cliente;

    public Conta(int numConta, Cliente cliente) {
        this.numConta = numConta;
        this.cliente = cliente;
        cliente.addConta(this);
    }

    // Getters e Setters
    public int getNumConta() {
        return numConta;
    }

    public void setNumConta(int numConta) {
        this.numConta = numConta;
    }

    public Cliente getCliente() {
        return cliente;
    }

    public void setCliente(Cliente cliente) {
        this.cliente = cliente;
    }
}

## 6. Teste
### teste Cliente

package hotel;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ClienteTest {

    private Cliente cliente;
    private Recepcionista recepcionista;
    private Quarto quarto;

    @BeforeEach
    void setUp() {
        // Um cliente precisa de um quarto, que precisa de uma recepcionista
        // Vamos criar todos os objetos necessários para os testes
        cliente = new Cliente("Ana Silva", 987654321, 12345678);
        recepcionista = new Recepcionista("Carlos Rocha", 911223344, 87654321);
        quarto = new Quarto(101, "Ala Sul", recepcionista);
    }

    @Test
    @DisplayName("Deve criar Cliente e verificar atributos herdados de Pessoa")
    void testCriacaoClienteEHeranca() {
        assertNotNull(cliente);
        assertEquals("Ana Silva", cliente.getNome());
        assertEquals(987654321, cliente.getNumCel());
        assertEquals(12345678, cliente.getRg());
    }

    @Test
    @DisplayName("Cliente deve ter listas de contas e quartos vazias ao ser criado")
    void testListasIniciaisVazias() {
        assertTrue(cliente.getContas().isEmpty(), "Lista de contas deve iniciar vazia");
        assertTrue(cliente.getQuartos().isEmpty(), "Lista de quartos deve iniciar vazia");
    }

    @Test
    @DisplayName("Deve fazer check-in de um cliente em um quarto")
    void testCheckIn() {
        // Estado inicial: quarto vago
        assertNull(quarto.getCliente());

        // Ação: Fazer check-in
        cliente.checkIn(quarto);

        // Verificação: O quarto agora está associado ao cliente
        assertEquals(cliente, quarto.getCliente(), "Quarto deve registrar o cliente após check-in");
        
        // Verificação: O cliente agora está associado ao quarto
        assertTrue(cliente.getQuartos().contains(quarto), "Cliente deve registrar o quarto após check-in");
        assertEquals(1, cliente.getQuartos().size());
    }

    @Test
    @DisplayName("Deve fazer check-out de um cliente de um quarto")
    void testCheckOut() {
        // Setup: Cliente faz check-in primeiro
        cliente.checkIn(quarto);
        assertEquals(cliente, quarto.getCliente()); // Garante que o check-in funcionou

        // Ação: Fazer check-out
        cliente.checkOut(quarto);

        // Verificação: O quarto deve ficar vago (cliente nulo)
        assertNull(quarto.getCliente(), "Quarto deve ficar vago (cliente null) após check-out");
    }

    @Test
    @DisplayName("Método pagarConta deve executar sem erros")
    void testPagarConta() {
        // Como o método só imprime no console, apenas testamos se ele executa sem lançar exceção
        Conta conta = new Conta(901, cliente);
        assertDoesNotThrow(() -> cliente.pagarConta(conta));
    }
}

### Teste recepcionista

package hotel;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class RecepcionistaTest {

    private Recepcionista recepcionista;
    private Cliente cliente;

    @BeforeEach
    void setUp() {
        recepcionista = new Recepcionista("Carlos Rocha", 911223344, 87654321);
        cliente = new Cliente("Ana Silva", 987654321, 12345678);
    }

    @Test
    @DisplayName("Deve criar Recepcionista e verificar atributos herdados")
    void testCriacaoRecepcionistaEHeranca() {
        assertNotNull(recepcionista);
        assertEquals("Carlos Rocha", recepcionista.getNome());
        assertEquals(911223344, recepcionista.getNumCel());
        assertEquals(87654321, recepcionista.getRg());
        assertTrue(recepcionista.getQuartosGerenciados().isEmpty());
    }

    @Test
    @DisplayName("Deve gerar uma conta para um cliente")
    void testGerarConta() {
        // Ação: Recepcionista gera uma conta
        Conta conta = recepcionista.gerarConta(cliente);

        // Verificação: A conta foi criada
        assertNotNull(conta);
        
        // Verificação: A conta está corretamente associada ao cliente (+1)
        assertEquals(cliente, conta.getCliente());

        // Verificação: O cliente está corretamente associado à conta (0..*)
        // (O construtor da Conta já faz essa associação)
        assertTrue(cliente.getContas().contains(conta));
        assertEquals(1, cliente.getContas().size());
    }

    @Test
    @DisplayName("Deve associar um quarto gerenciado à recepcionista")
    void testAddQuartoGerenciado() {
        // Setup: Quarto é criado (já associado à recepcionista pelo construtor)
        Quarto quarto = new Quarto(202, "Ala Norte", recepcionista);
        
        // Ação: Adicionar o quarto à lista da recepcionista
        // Nota: O diagrama (1..*) implica que a lista deve ser preenchida.
        // Meu código sugere que isso é feito manualmente ou pelo construtor do Quarto.
        // Vamos testar o método 'addQuartoGerenciado'
        
        recepcionista.addQuartoGerenciado(quarto);

        // Verificação: Associação de Recepcionista -> Quarto (1..*)
        assertTrue(recepcionista.getQuartosGerenciados().contains(quarto));
        assertEquals(1, recepcionista.getQuartosGerenciados().size());

        // Verificação: Associação de Quarto -> Recepcionista (+1)
        assertEquals(recepcionista, quarto.getRecepcionista());
    }
}

### teste Quarto 

package hotel;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class QuartoTest {

    private Quarto quarto;
    private Recepcionista recepcionista;
    private Cliente cliente;

    @BeforeEach
    void setUp() {
        recepcionista = new Recepcionista("Carlos Rocha", 911223344, 87654321);
        quarto = new Quarto(101, "Ala Sul", recepcionista);
        cliente = new Cliente("Ana Silva", 987654321, 12345678);
    }

    @Test
    @DisplayName("Deve criar Quarto e verificar atributos")
    void testCriacaoQuarto() {
        assertNotNull(quarto);
        assertEquals(101, quarto.getNumQuarto());
        assertEquals("Ala Sul", quarto.getLocalizacao());
    }

    @Test
    @DisplayName("Quarto deve ter uma Recepcionista associada na criação (+1)")
    void testAssociacaoObrigatoriaRecepcionista() {
        // A associação +1 com Recepcionista é garantida pelo construtor
        assertNotNull(quarto.getRecepcionista());
        assertEquals(recepcionista, quarto.getRecepcionista());
    }

    @Test
    @DisplayName("Quarto deve iniciar vago (sem cliente) (0..1)")
    void testAssociacaoOpcionalCliente() {
        // A associação 0..1 com Cliente deve começar como null (0)
        assertNull(quarto.getCliente(), "Quarto deve ser criado vago (sem cliente)");
    }

    @Test
    @DisplayName("Deve permitir definir e remover um cliente do quarto (ocupar/liberar)")
    void testSetCliente() {
        // Ocupar o quarto
        quarto.setCliente(cliente);
        assertEquals(cliente, quarto.getCliente(), "Quarto deve estar associado ao cliente");

        // Liberar o quarto
        quarto.setCliente(null);
        assertNull(quarto.getCliente(), "Quarto deve estar vago (cliente null)");
    }
}

### teste conta

package hotel;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ContaTest {

    private Conta conta;
    private Cliente cliente;

    @BeforeEach
    void setUp() {
        cliente = new Cliente("Ana Silva", 987654321, 12345678);
        // A conta é criada e já associada ao cliente
        conta = new Conta(901, cliente);
    }

    @Test
    @DisplayName("Deve criar Conta e verificar atributos")
    void testCriacaoConta() {
        assertNotNull(conta);
        assertEquals(901, conta.getNumConta());
    }

    @Test
    @DisplayName("Conta deve ter um Cliente associado na criação (+1)")
    void testAssociacaoObrigatoriaCliente() {
        // A associação +1 com Cliente é garantida pelo construtor
        assertNotNull(conta.getCliente());
        assertEquals(cliente, conta.getCliente());
    }

    @Test
    @DisplayName("Cliente deve ter a Conta em sua lista após criação da Conta (0..*)")
    void testAssociacaoReversaClienteConta() {
        // O construtor da Conta (que eu escrevi) já adiciona a conta à lista do cliente
        assertTrue(cliente.getContas().contains(conta), "A lista de contas do cliente deve conter esta conta");
        assertEquals(1, cliente.getContas().size());

        // Testa adicionar uma segunda conta
        Conta conta2 = new Conta(902, cliente);
        assertTrue(cliente.getContas().contains(conta2));
        assertEquals(2, cliente.getContas().size());
    }
}

## 7. Outro Diagrama
<img width="1393" height="692" alt="image" src="https://github.com/user-attachments/assets/1f3c7480-f5b5-4e87-b37d-251563bdd327" />

## 8. Java respectivo
### Pessoa:

// Pessoa.java

import java.util.List;
import java.util.ArrayList;

public class Pessoa {

    protected String NomePes;
    protected String EndPes;
    protected String TelPes;
    protected double RenPes;
    protected boolean SitPes;
    protected boolean TipPes;
    
    protected List<ContaComum> contas;

    public Pessoa() {
        this.contas = new ArrayList<>();
    }
    
    // Getters e Setters
    public String getNomePes() {
        return NomePes;
    }

    public void setNomePes(String NomePes) {
        this.NomePes = NomePes;
    }

    public List<ContaComum> getContas() {
        return contas;
    }

    public void addConta(ContaComum conta) {
        this.contas.add(conta);
    }
}

### Fisica:

// Fisica.java

public class Fisica extends Pessoa {

    private long CPFPes;

    public void Gravar() {
        System.out.println("Gravando dados de Pessoa Física...");
    }

    public int ValCPF(long CPFPes) {
        System.out.println("Validando CPF: " + CPFPes);
        return 1; // 1 = Válido, 0 = Inválido
    }

    public int ConCPF(long CPFPes) {
        System.out.println("Consultando CPF: " + CPFPes);
        return 1; // 1 = Encontrado, 0 = Não encontrado
    }

    // Getters e Setters
    public long getCPFPes() {
        return CPFPes;
    }

    public void setCPFPes(long CPFPes) {
        this.CPFPes = CPFPes;
    }
}

### juridica:

//  Juridica.java

public class Juridica extends Pessoa {

    private long CNPJur;

    public void Gravar() {
        System.out.println("Gravando dados de Pessoa Jurídica...");
    }

    public int ValCNPJ(long CNPJur) {
        System.out.println("Validando CNPJ: " + CNPJur);
        return 1; // 1 = Válido, 0 = Inválido
    }

    public int ConCNPJ(long CNPJur) {
        System.out.println("Consultando CNPJ: " + CNPJur);
        return 1; // 1 = Encontrado, 0 = Não encontrado
    }

    // Getters e Setters
    public long getCNPJur() {
        return CNPJur;
    }

    public void setCNPJur(long CNPJur) {
        this.CNPJur = CNPJur;
    }
}

### Historico

// Historico.java

import java.util.Date;

public class Historico {

    private long NroConta;
    private boolean TipoHis; // true = Crédito, false = Débito
    private Date DtaHis;

    // Construtor para facilitar a criação de um histórico
    public Historico(long nroConta, boolean tipoHis, Date dtaHis) {
        this.NroConta = nroConta;
        this.TipoHis = tipoHis;
        this.DtaHis = dtaHis;
    }

    public int Gravar(long NroConta, boolean TipoHis, double ValorHis) {
        System.out.println("Gravando histórico para conta " + NroConta + " | Valor: " + ValorHis);
        // Lógica para salvar a transação no banco de dados, por exemplo...
        return 1; // 1 = Sucesso
    }

    public int Extrato(long NroConta, Date Petrin, Date PerFin) {
        System.out.println("Gerando extrato para conta " + NroConta + " de " + Petrin + " até " + PerFin);
        // Lógica para buscar e formatar o extrato...
        return 1; // 1 = Sucesso
    }

    // Getters e Setters
    public long getNroConta() {
        return NroConta;
    }

    public boolean isTipoHis() {
        return TipoHis;
    }

    public Date getDtaHis() {
        return DtaHis;
    }
}

### ContaComum

// ContaComum.java

import java.util.Date;
import java.util.List;
import java.util.ArrayList;

public abstract class ContaComum {

    protected long NroConta;
    protected double Saldo;
    protected Date DtAber;
    protected int Tipo;
    protected Date DtEncer;
    protected boolean Situacao; // true = Ativa, false = Encerrada
    protected int Senha;
    
    protected List<Pessoa> titulares;
    
    protected List<Historico> historicos;

    public ContaComum() {
        this.titulares = new ArrayList<>();
        this.historicos = new ArrayList<>();
        this.DtAber = new Date(); // Define a data de abertura como hoje
        this.Situacao = true;
    }

    // Métodos 
    public long Abertura(double Saldo, boolean Tipo, int Senha) {
        this.Saldo = Saldo;
        this.Tipo = (Tipo ? 1 : 0); 
        this.Senha = Senha;
        this.NroConta = (long) (Math.random() * 100000); // Exemplo de geração de num. conta
        System.out.println("Conta " + this.NroConta + " aberta com saldo R$" + Saldo);
        return this.NroConta;
    }

    public void Encerramento() {
        this.Situacao = false;
        this.DtEncer = new Date();
        System.out.println("Conta " + NroConta + " encerrada.");
    }

    public int Saque(double Valor) {
        if (this.Situacao && this.Saldo >= Valor) {
            this.Saldo -= Valor;
            this.historicos.add(new Historico(this.NroConta, false, new Date()));
            System.out.println("Saque de R$" + Valor + " realizado. Novo saldo: R$" + this.Saldo);
            return 1; // 1 = Sucesso
        }
        System.out.println("Saque de R$" + Valor + " falhou. Saldo insuficiente ou conta inativa.");
        return 0; // 0 = Falha
    }

    public int Deposito(long NroConta, double Valor) {
        if (this.NroConta == NroConta && this.Situacao) {
            this.Saldo += Valor;
            this.historicos.add(new Historico(this.NroConta, true, new Date()));
            System.out.println("Depósito de R$" + Valor + " realizado. Novo saldo: R$" + this.Saldo);
            return 1; // 1 = Sucesso
        }
        return 0; // 0 = Falha (conta não encontrada ou inativa)
    }

    public double VerSaldo() {
        return this.Saldo;
    }

    public int Consulta(long NroConta) {
        // Método para verificar se a conta existe.
        if (this.NroConta == NroConta) {
            System.out.println("Consulta à conta " + NroConta + " bem-sucedida.");
            System.out.println("Saldo: R$" + this.Saldo);
            System.out.println("Situação: " + (this.Situacao ? "Ativa" : "Inativa"));
            return 1; // 1 = Encontrada
        }
        return 0; // 0 = Não encontrada
    }

    public int ValSenha(int Senha) {
        if (this.Senha == Senha) {
            return 1; // 1 = Senha correta
        }
        return 0; // 0 = Senha incorreta
    }

    // Getters e Setters
    public long getNroConta() {
        return NroConta;
    }
}

### ContaEspecial

// ContaEspecial.java

public class ContaEspecial extends ContaComum {

    private double Limite;
    
    @Override
    public long Abertura(double Saldo, boolean Tipo, int Senha) {
        super.Abertura(Saldo, Tipo, Senha); 
        this.Limite = 1000.00; 
        System.out.println("Conta Especial criada com limite de R$" + this.Limite);
        return this.NroConta;
    }

    @Override
    public int Saque(double Valor) {
        if (this.Situacao && (this.Saldo + this.Limite) >= Valor) {
            this.Saldo -= Valor;
            this.historicos.add(new Historico(this.NroConta, false, new Date()));
            System.out.println("Saque (Especial) de R$" + Valor + " realizado. Novo saldo: R$" + this.Saldo);
            return 1; // 1 = Sucesso
        }
        System.out.println("Saque (Especial) de R$" + Valor + " falhou. Limite excedido.");
        return 0; // 0 = Falha
    }

    // Getters e Setters
    public double getLimite() {
        return Limite;
    }

    public void setLimite(double limite) {
        this.Limite = limite;
    }
}

### Poupança

// ContaPoupanca.java

import java.util.Date;

public class ContaPoupanca extends ContaComum {

    public void Rendimento(Date DtDia, double Juros) {
        double rendimento = this.Saldo * Juros;
        this.Saldo += rendimento;
        
        System.out.println("Rendimento de R$" + rendimento + " aplicado na data " + DtDia);
        System.out.println("Novo saldo: R$" + this.Saldo);
        
        this.historicos.add(new Historico(this.NroConta, true, DtDia));
    }
}

## 9. Teste

### teste pessoa fisica

// FisicaTest.java

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class FisicaTest {

    private Fisica pessoaFisica;

    @BeforeEach
    void setUp() {
        pessoaFisica = new Fisica();
        pessoaFisica.setNomePes("Carlos Souza");
        pessoaFisica.setEndPes("Rua A, 123");
        pessoaFisica.setCPFPes(98765432100L);
    }

    @Test
    void testHerancaPessoa() {
        assertEquals("Carlos Souza", pessoaFisica.getNomePes());
        assertEquals("Rua A, 123", pessoaFisica.EndPes);
    }

    @Test
    void testAtributosFisica() {
        assertEquals(98765432100L, pessoaFisica.getCPFPes());
    }

    @Test
    void testMetodosValidacaoStub() {
        assertEquals(1, pessoaFisica.ValCPF(123L));
        assertEquals(1, pessoaFisica.ConCPF(123L));
    }
}

### teste pessoa juridica

// JuridicaTest.java

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class JuridicaTest {

    private Juridica pessoaJuridica;

    @BeforeEach
    void setUp() {
        pessoaJuridica = new Juridica();
        pessoaJuridica.setNomePes("Empresa XYZ Ltda");
        pessoaJuridica.setEndPes("Av. B, 456");
        pessoaJuridica.setCNPJur(11222333000144L);
    }

    @Test
    void testHerancaPessoa() {
        assertEquals("Empresa XYZ Ltda", pessoaJuridica.getNomePes());
        assertEquals("Av. B, 456", pessoaJuridica.EndPes);
    }

    @Test
    void testAtributosJuridica() {
        assertEquals(11222333000144L, pessoaJuridica.getCNPJur());
    }

    @Test
    void testMetodosValidacaoStub() {
        assertEquals(1, pessoaJuridica.ValCNPJ(123L));
        assertEquals(1, pessoaJuridica.ConCNPJ(123L));
    }
}

### teste historico 

// HistoricoTest.java

import org.junit.jupiter.api.Test;
import java.util.Date;
import static org.junit.jupiter.api.Assertions.*;

class HistoricoTest {

    @Test
    void testConstrutorHistorico() {
        Date dataAgora = new Date();
        Historico hist = new Historico(901, true, dataAgora); 

        assertEquals(901, hist.getNroConta());
        assertTrue(hist.isTipoHis());
        assertEquals(dataAgora, hist.getDtaHis());
    }

    @Test
    void testMetodosStub() {
        Historico hist = new Historico(1, true, new Date());
        
        assertEquals(1, hist.Gravar(1, true, 100.0));
        assertEquals(1, hist.Extrato(1, new Date(), new Date()));
    }
}

### teste conta especial

// ContaEspecialTest.java

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ContaEspecialTest {

    private ContaEspecial contaEspecial;

    @BeforeEach
    void setUp() {
        contaEspecial = new ContaEspecial();
        //limite padrão de 1000.0
        contaEspecial.Abertura(500.0, true, 1234); 
    }

    @Test
    void testAberturaContaEspecial() {
        assertEquals(500.0, contaEspecial.VerSaldo());
        assertEquals(1000.0, contaEspecial.getLimite()); // Verifica o limite
    }

    @Test
    void testSaqueSimplesEspecial() {
        int resultado = contaEspecial.Saque(300.0); // Saldo é suficiente

        assertEquals(1, resultado);
        assertEquals(200.0, contaEspecial.VerSaldo());
        assertEquals(1, contaEspecial.historicos.size());
    }

    @Test
    void testSaqueUsandoLimite() {
        // Saldo 500, Limite 1000. Total disponível = 1500
        int resultado = contaEspecial.Saque(800.0); 

        assertEquals(1, resultado);
        // Saldo fica negativo
        assertEquals(-300.0, contaEspecial.VerSaldo()); 
        assertEquals(1, contaEspecial.historicos.size());
        assertFalse(contaEspecial.historicos.get(0).isTipoHis()); // Débito
    }
    
    @Test
    void testSaqueExatamenteNoLimite() {
        // Saldo 500, Limite 1000
        int resultado = contaEspecial.Saque(1500.0); 

        assertEquals(1, resultado);
        assertEquals(-1000.0, contaEspecial.VerSaldo()); 
        assertEquals(1, contaEspecial.historicos.size());
    }

    @Test
    void testSaqueFalhaExcedendoLimite() {
        // Saldo 500, Limite 1000. Total 1500. Tenta sacar 1501
        int resultado = contaEspecial.Saque(1501.0); 

        assertEquals(0, resultado);
        // Saldo não muda
        assertEquals(500.0, contaEspecial.VerSaldo()); 
        assertEquals(0, contaEspecial.historicos.size());
    }
}

### teste conta Poupança

// ContaPoupancaTest.java

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import java.util.Date;
import static org.junit.jupiter.api.Assertions.*;

class ContaPoupancaTest {

    private ContaPoupanca conta;
    private Fisica titular;

    @BeforeEach
    void setUp() {
        // Cria uma nova conta e um titular para cada teste
        conta = new ContaPoupanca();
        conta.Abertura(500.0, true, 1234); // Saldo inicial de 500, senha 1234

        titular = new Fisica();
        titular.setNomePes("Ana Silva");
        titular.setCPFPes(12345678900L);
        
        // Simula a associação
        conta.titulares.add(titular);
        titular.addConta(conta);
    }

    @Test
    void testAberturaConta() {
        assertTrue(conta.getNroConta() > 0);
        assertEquals(500.0, conta.VerSaldo());
        assertTrue(conta.Situacao);
        assertEquals(0, conta.historicos.size()); // Abertura não gera histórico
    }

    @Test
    void testDepositoSucesso() {
        int resultado = conta.Deposito(conta.getNroConta(), 200.0);
        
        assertEquals(1, resultado);
        assertEquals(700.0, conta.VerSaldo());
        assertEquals(1, conta.historicos.size());
        assertTrue(conta.historicos.get(0).isTipoHis()); // true = Crédito
    }

    @Test
    void testSaqueSucesso() {
        int resultado = conta.Saque(300.0);

        assertEquals(1, resultado);
        assertEquals(200.0, conta.VerSaldo());
        assertEquals(1, conta.historicos.size());
        assertFalse(conta.historicos.get(0).isTipoHis()); // false = Débito
    }

    @Test
    void testSaqueFalhaSaldoInsuficiente() {
        int resultado = conta.Saque(600.0); // Saldo é 500

        assertEquals(0, resultado);
        assertEquals(500.0, conta.VerSaldo());
        assertEquals(0, conta.historicos.size());
    }

    @Test
    void testEncerramentoConta() {
        assertNotNull(conta.DtAber);
        assertNull(conta.DtEncer);
        assertTrue(conta.Situacao);

        conta.Encerramento();

        assertFalse(conta.Situacao);
        assertNotNull(conta.DtEncer);
    }

    @Test
    void testSaqueFalhaContaEncerrada() {
        conta.Encerramento();
        int resultado = conta.Saque(100.0);

        assertEquals(0, resultado);
        assertEquals(500.0, conta.VerSaldo()); // Saldo não muda
    }

    @Test
    void testMetodoRendimento() {
        // Teste específico da ContaPoupanca
        conta.Rendimento(new Date(), 0.10); // 10% de juros
        
        // 500 * 0.10 = 50. Novo saldo = 550
        assertEquals(550.0, conta.VerSaldo());
        assertEquals(1, conta.historicos.size());
        assertTrue(conta.historicos.get(0).isTipoHis()); // Rendimento é um crédito
    }

    @Test
    void testValSenha() {
        assertEquals(1, conta.ValSenha(1234));
        assertEquals(0, conta.ValSenha(9999));
    }

    @Test
    void testRelacionamentoContaPessoa() {
        assertEquals(1, conta.titulares.size());
        assertEquals("Ana Silva", conta.titulares.get(0).getNomePes());
        assertEquals(1, titular.getContas().size());
        assertEquals(conta.getNroConta(), titular.getContas().get(0).getNroConta());
    }
}



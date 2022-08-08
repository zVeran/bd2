# bd2

drop database dbDistribuidoraEduBia;
create database dbDistribuidoraEduBia;
use dbDistribuidoraEduBia;

CREATE TABLE tbUF (
IdUF INT AUTO_INCREMENT PRIMARY KEY,
UF CHAR(2) UNIQUE not null
);

CREATE TABLE tbBairro (
IdBairro INT AUTO_INCREMENT PRIMARY KEY,
Bairro VARCHAR(200) not null
);

CREATE TABLE tbCidade (
IdCidade INT AUTO_INCREMENT PRIMARY KEY,
Cidade VARCHAR(200) not null
);

CREATE TABLE tbEndereco (
CEP VARCHAR(8) PRIMARY KEY,
Logradouro VARCHAR(200),
IdBairro INT,
FOREIGN KEY (IdBairro)
REFERENCES tbBairro (IdBairro),
IdCidade INT,
FOREIGN KEY (IdCidade)
REFERENCES tbCidade (IdCidade),
IdUF INT,
FOREIGN KEY (IdUF)
REFERENCES tbUF (IdUF)
);

CREATE TABLE tbCliente (
Id INT PRIMARY KEY AUTO_INCREMENT,
Nome VARCHAR(50) NOT NULL,
CEP VARCHAR(8) NOT NULL,
CompEnd VARCHAR(50),
NumEnd int null,
FOREIGN KEY (CEP)
REFERENCES tbEndereco (CEP)
);

CREATE TABLE tbClientePF (
IdCliente INT AUTO_INCREMENT,
FOREIGN KEY (IdCliente)
REFERENCES tbCliente (Id),
Cpf VARCHAR(11) NOT NULL PRIMARY KEY,
Rg VARCHAR(8),
RgDig VARCHAR(1),
Nasc DATE
);

CREATE TABLE tbClientePJ (
IdCliente INT AUTO_INCREMENT,
FOREIGN KEY (IdCliente)
REFERENCES tbCliente (Id),
Cnpj VARCHAR(14) NOT NULL PRIMARY KEY,
Ie VARCHAR(9)
);

CREATE TABLE tbNotaFiscal (
NF INT PRIMARY KEY,
TotalNota DECIMAL(7 , 2 ) NOT NULL,
DataEmissao DATE NOT NULL
);

CREATE TABLE tbFornecedor (
Codigo INT PRIMARY KEY AUTO_INCREMENT,
Cnpj VARCHAR(14) NOT NULL,
Nome VARCHAR(200),
Telefone VARCHAR(11)
);

CREATE TABLE tbCompra (
NotaFiscal INT PRIMARY KEY,
DataCompra DATE NOT NULL,
ValorTotal DECIMAL(8 , 2 ) NOT NULL,
QtdTotal bigint NOT NULL,
Cod_Fornecedor INT,
FOREIGN KEY (Cod_Fornecedor)
REFERENCES tbFornecedor (Codigo)
);

CREATE TABLE tbProduto (
CodBarras bigint PRIMARY KEY,
Qtd numeric(6,2),
Nome VARCHAR(50),
ValorUnitario DECIMAL(6 ,2) NOT NULL
);

CREATE TABLE tbItemCompra (
Qtd bigint NOT NULL,
ValorItem DECIMAL(6 , 2 ) NOT NULL,
NotaFiscal INT,
CodBarras bigint,
PRIMARY KEY (NotaFiscal , CodBarras),
FOREIGN KEY (NotaFiscal)
REFERENCES tbCompra (NotaFiscal),
FOREIGN KEY (CodBarras)
REFERENCES tbProduto (CodBarras)
);

CREATE TABLE tbVenda (
IdCliente INT,
FOREIGN KEY (IdCliente)
REFERENCES tbCliente (Id),
NumeroVenda INT AUTO_INCREMENT PRIMARY KEY,
DataVenda DATE NOT NULL,
TotalVenda DECIMAL(7 , 2 ) NOT NULL,
NotaFiscal INT,
FOREIGN KEY (NotaFiscal)
REFERENCES tbNotaFiscal (NF)
);

CREATE TABLE tbItemVenda (
NumeroVenda INT,
CodBarras bigint,
PRIMARY KEY (NumeroVenda , CodBarras),
FOREIGN KEY (NumeroVenda)
REFERENCES tbVenda (NumeroVenda),
FOREIGN KEY (CodBarras)
REFERENCES tbProduto (CodBarras),
Qtd bigint,
ValorItem DECIMAL(6 , 2 )
);



-- exercicio 1



insert into tbFornecedor( Cnpj, Nome, Telefone)
values('1245678937123','Revenda Chico Loco','11934567897');
insert into tbFornecedor( Cnpj, Nome, Telefone)
values('1345678937123','José Faz Tudo S/A','11934567898');
insert into tbFornecedor( Cnpj, Nome, Telefone)
values('1445678937123','Vadalto Entregas','11934567899');
insert into tbFornecedor( Cnpj, Nome, Telefone)
values('1545678937123','Astrogildo das Estreça','11934567800');
insert into tbFornecedor( Cnpj, Nome, Telefone)
values('1645678937123','Amoroso e Doce','11934567801');
insert into tbFornecedor(Cnpj, Nome, Telefone)
values('1745678937123','Marcelo Dedal','11934567802');
insert into tbFornecedor(Cnpj, Nome, Telefone)
values('1845678937123','Franciscano Cachaça','11934567803');
insert into tbFornecedor(Cnpj, Nome, Telefone)
values('1945678937123','Joãozinho Chupeta','11934567804');



select * from tbFornecedor;



-- exercicio 2



delimiter $$
create procedure SpInsertCidade(vCidade varchar(200))
begin
insert into tbCidade(cidade) values (vCidade);
end $$

call spInsertCidade("Rio de janeiro");
call spInsertCidade("São Carlos");
call spInsertCidade("Campinas");
call spInsertCidade("Franco da Rocha");
call spInsertCidade("Osasco");
call spInsertCidade("Pirituba");
call spInsertCidade("Lapa");
call spInsertCidade("Ponta Grossa");

select * from tbcidade;


-- exercicio 3


delimiter $$
create procedure spInsertUF(vEstado char(2))
begin
insert into tbUF(UF) values (vEstado);
end $$

call spInsertUF("SP");
call spInsertUF("RJ");
call spInsertUF("RS");

select * from tbUF;



-- exercicio 4



delimiter $$
create procedure SpInsertBairro(vBairro varchar(200))
begin
insert into tbBairro(Bairro) values (vBairro);
end $$



call spInsertBairro("Aclimação");
call spInsertBairro("Capão Redondo");
call spInsertBairro("Pirituba");
call spInsertBairro("Liberdade");



select * from tbbairro;



-- exercicio 5

delimiter $$
create procedure spInsertProduto(vCodigoBarras bigint, vNome varchar(200),vValorUnitario decimal(5, 2), vQtd int)
begin
insert into tbProduto (CodBarras, Nome, ValorUnitario, Qtd) values (vCodigoBarras, vNome, vValorUnitario, vQtd);
end $$

call spInsertProduto("12345678910111", "Rei de Papel Machi", 54.61, 120);
call spInsertProduto("12345678910112", "Bolinha de Sabão", "100.45", "120");
call spInsertProduto("12345678910113", "Carro batebate", "44.00", "120");
call spInsertProduto("12345678910114", "bola furada", "10.00", "120");
call spInsertProduto("12345678910115", "maça laranja", "99.44", "120");
call spInsertProduto("12345678910116", "boneco do hitler", "124.00", "200");
call spInsertProduto("12345678910117", "farinha de surui", "50.00", "200");
call spInsertProduto("12345678910118", "zelador de cemitério", "24.50", "100");



select * from tbproduto;


-- exercicio 6


delimiter $$
create procedure spInsertEndereco(vCEP varchar(8), vLogradouro varchar(200), vBairro varchar(200), vCidade varchar(200), vUF char(2))
begin 

declare idBairroSP int;
declare idCidadeSP int;
declare idUFSP int;
declare mensgErro text;

if not exists (select * from tbBairro where Bairro = vBairro) then
	insert into tbbairro (bairro) values (vBairro);
end if;
 
if not exists (select * from tbCidade where Cidade = vCidade) then
	insert into tbcidade (cidade) values (vCidade);
end if;

if not exists (select * from tbUF where  UF = vUF) then
	insert into tbuf (uf) values (vUF);
end if;


if not exists (select * from tbEndereco where vCEP = CEP) then

	set @idBairroSP = (select idBairro from tbBairro where Bairro = vBairro);
	set @idCidadeSP = (select idCidade from tbCidade where Cidade = vCidade);
	set @idUFSP = (select idUF from tbUF where UF = vUF);
    
		else
    select 'Já existe';
end if;


insert into tbEndereco(CEP, Logradouro, IdBairro, IdCidade, IdUF) values (vCEP, vLogradouro, @idBairroSP, @idCidadeSP, @idUFSP);
end $$ 


call spInsertEndereco(12345050, "Rua da Federal", "Lapa", "São Paulo", "SP");
call spInsertEndereco(12345051, "Av Brasil", "Lapa", "Campinas", "SP");
call spInsertEndereco(12345052, "Rua Liberdade", "Consolação", "São Paulo", "SP");
call spInsertEndereco(12345053, "Av Paulista", "Penha", "Rio de Janeiro", "RJ");
call spInsertEndereco(12345054, "Rua Ximbú", "Penha", "Rio de Janeiro", "RJ");
call spInsertEndereco(12345055, "Rua Piu X1", "Penha", "Campinas", "SP");
call spInsertEndereco(12345056, "Rua chocolate", "Aclimação", "Barra Mansa", "RJ");
call spInsertEndereco(12345057, "Rua Pão na Chapa", "Barra Funda", "Ponta Grossa", "RS");

select * from tbEndereco;
select * from tbBairro;
select * from tbCidade;
select * from tbUF;

-- exercicio 7 


delimiter $$
create procedure spInsertcliPF (Nomecli varchar(200), NumEnd varchar(5), CompEnd varchar(50), CEP varchar(8), CPF varchar(11), RG varchar(9), RG_Dig varchar(1), Nasc Date, Logradouro varchar(50),
Bairro varchar(50), Cidade varchar (50), UF char(2))

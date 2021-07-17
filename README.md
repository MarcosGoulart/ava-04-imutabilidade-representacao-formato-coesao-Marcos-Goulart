# Avaliação 04 Imutabilidade, representação & formato e coesão.

Link do Classroom: <https://classroom.github.com/a/qvoQZf9k>

[strong, firm, focus, always concentrate ... everything is OOP](http://youtu.be/PWiipjG7NEU)

Disciplina `30%` concluída, [ava-02](cat/0.gif), [ava-03](cat/1.gif), [ava-04](cat/2.gif) 👈 estás aqui. [Na metade da disciplina](cat/3.gif), [nas últimas semanas](cat/4.gif), e [após tudo acabado!](cat/5.gif).


## Implementar e testar segundo as especificações

- Esta atividade é avaliada com esforço estimado entre 6 e 12h.
- Copie os casos de teste para o método `main` em [App.java](src/App.java), conforme o exemplo que já está no arquivo. Comente com `//` ou `/*` e `*/` as linhas que ainda não foram implementadas.
- Os Casos de Teste podem ser corrigidos se estiverem mal escritos, mas **a especificação dos objetos não pode ser alterada**.
- E, por fim, assegure-se de **assistir as videoaulas antes de começar**, pois lá estão explicados todos os conceitos e práticas presentes nesta atividade.



### Implementar uma classe para gerar objetos imutáveis de Tempo e suas operações básicas

Considere um instante no tempo em horas, minutos e segundos, entre `00:00:00` e `23:59:59`. Implementar construtores e métodos para lidar com esse tempo de maneira *fail-safe* (sem rejeitar as entradas, mas adaptando-as). A API (interface do objeto) será implementada na língua inglesa com construtores para `h:m:s`, `h:m` e somente `h`. Os métodos disponíveis serão `Time#plus(Time):Time`, `Time#plusHours(int):Time`, `Time#plusMinutes(int):Time`, `Time#plusSeconds(int):Time`, `Time#minus(Time):Time`, `Time#minusHours(int):Time`, `Time#minusMinutes(int):Time`, `Time#minusSeconds(int):Time`, `Time#tick():Time` (adiciona um segundo), `Time#shift():Time` (inverte o turno),`Time#isMidDay():boolean` (questiona se é meio-dia) e `Time#isMidNight():boolean` (questiona se é meia-noite).

Casos de teste:

```java
Time t1 = new Time();
// representação string, padrão 00:00:00
System.out.println(t1.toString().equals("00:00:00"));
Time t2 = new Time(1, 40, 5);
System.out.println(t2.toString().equals("01:40:05"));
Time t3 = t1.plus(t2);
System.out.println(t3.toString().equals("01:40:05"));
System.out.println(t3.hours() == 1);
System.out.println(t3.minutes() == 40);
System.out.println(t3.seconds() == 5);
// deve ser imutável
System.out.println(t1.hours() == 0);
System.out.println(t1.minutes() == 0);
System.out.println(t1.seconds() == 0);
// plus
Time t4 = t3.plus(t2);
System.out.println(t4.toString().equals("03:20:10"));
// implementar equals
System.out.println(t4.equals(new Time(3, 20, 10)));
Time t5 = t2.plusHours(1);
System.out.println(t5.toString().equals("02:40:05"));
Time t6 = t4.plusHours(23);
System.out.println(t6.toString().equals("02:20:10"));
Time t7 = t6.plusMinutes(10);
System.out.println(t7.toString().equals("02:30:10"));
Time t8 = t7.plusSeconds(80);
System.out.println(t8.toString().equals("02:31:30"));
Time t9 = new Time().plusHours(19).plusMinutes(23).plusSeconds(18);
System.out.println(t9.toString().equals("19:23:18"));
Time t10 = t9.plusHours(-1).plusMinutes(-1).plusSeconds(-1);
System.out.println(t10.toString().equals("18:22:17"));
Time t11 = t10.minusHours(2).minusMinutes(2).minusSeconds(2);
System.out.println(t11.toString().equals("16:20:15"));
Time t12 = t11.minusHours(-5);
System.out.println(t12.toString().equals("21:20:15"));
Time t13 = t11.minus(t12);
System.out.println(t13.toString().equals("19:00:00"));
System.out.println(t13.isMidDay() == false);
Time t14 = t13.minus(t13);
System.out.println(t14.toString().equals("00:00:00"));
System.out.println(t14.isMidDay() == false);
System.out.println(t14.isMidNight() == true);
System.out.println(t14.plusHours(12).isMidDay() == true);
Time t15 = new Time(3, 40);
System.out.println(t15.toString().equals("03:40:00"));
Time t16 = t15.shift();
System.out.println(t16.toString().equals("15:40:00"));
Time t17 = t16.shift();
System.out.println(t17.toString().equals("03:40:00"));
Time t18 = t17.tick();
System.out.println(t18.toString().equals("03:40:01"));
Time t19 = t18.tick().tick().tick();
System.out.println(t19.toString().equals("03:40:04"));
Time t20 = t19.plusHours(50).plusMinutes(50).minusSeconds(50).tick().shift();
System.out.println(t20.toString().equals("quanto vale t20? escreva aqui"));
```

**Desafio: a classe `Time` com apenas um atributo `int` em vez de três [h,m,s].**



### Representações e Formatos de `Time`

Implementar os métodos de conversão para `Time` conforme os casos de teste.

```java
Time tr1 = new Time(9, 40, 15);
// representação string, padrão 00:00:00
System.out.println(tr1.toString().equals("09:40:15"));
// representação string com formato alternativo
System.out.println(tr1.toLongString().equals("9 horas 40 minutos e 15 segundos"));
// fromString, formato 00:00:00
Time tr2 = Time.fromString("01:36:00");
System.out.println(tr2.toLongString().equals("1 hora e 36 minutos"));
// fromDouble
Time tr3 = Time.fromDouble(3.824);
System.out.println(tr3.toLongString().equals("3 horas 49 minutos e 26 segundos"));
// sem arredondamentos
System.out.println(Time.fromDouble(17.1447).toLongString().equals("17 horas 8 minutos e 40 segundos"));
// fromSeconds
Time tr4 = Time.fromSeconds(76632);
// System.out.println(tr4.toLongString().equals("21 horas 15 minutos e 32 segundos")); // PATCH:
System.out.println(tr4.toLongString().equals("21 horas 17 minutos e 12 segundos"));
// --------------------------------------------------------------------------------
System.out.println(Time.fromSeconds(68400).toLongString().equals("19 horas"));
// toDouble

// PATCH
// Time tr4 = Time.fromString("16:45:11"); // DEVE SER: (causa erro por redeclaração de tr4)
tr4 = Time.fromString("16:45:11"); // 👍

System.out.println(tr4.toDouble()); // 16.75305556 aproximadamente
System.out.println(Time.fromString("13:04:59").toDouble()); // 13.08305556 aproximadamente

double tr5double = Time.fromString("13:04:59").toDouble();
Time tr5 = Time.fromDouble(tr5double);
System.out.println(tr5.toLongString().equals("13 horas 4 minutos e 59 segundos"));
// fromTime
Time tr6 = Time.from(tr5);
// toShortString
System.out.println(tr6.toShortString().equals("13h04m59s"));
System.out.println(Time.fromString("15:03:00").toShortString().equals("15h03m"));
// System.out.println(Time.fromString("05:00:01").toShortString().equals("15h00m01s")); // PATCHED:
System.out.println(Time.fromString("15:00:01").toShortString().equals("15h00m01s"));
// constantes
Time tr7 = Time.MIDDAY;
System.out.println(tr7.toShortString().equals("12h"));
Time tr8 = Time.MIDNIGHT;
System.out.println(tr8.toShortString().equals("00h"));
System.out.println(Time.MIDDAY.toInt() == 43200);
System.out.println(Time.MIDNIGHT.toInt() == 0);
```



### Implementar o objeto `LatLong` para a representação de coordenadas geográficas

Instâncias de `LatLong` devem representar uma posição geográfica no formato de _latitude_ e _longitude_ em graus decimais, sendo que a _latitude_ vai de `-90.0` a `+90.0` e a _longitude_ de `-180.0` a `+180.0`. A construção sem argumentos de uma coordenada deve instanciar _latitude_ `0` e _longitude_ `0`. Após a construção não devem ser permitidas alterações na _latitude_ e _longitude_ a não ser que outra instância seja construída, em outras palavras, os objetos devem ser **imutáveis**.

Considere os Casos de Teste:

```java
// construtores:
LatLong c1 = new LatLong();
System.out.println(c1.latitude == 0.0);
System.out.println(c1.longitude == 0.0);

LatLong c2 = new LatLong(50.0, 134.0);
System.out.println(c2.latitude == 50.0);
System.out.println(c2.longitude == 134.0);

LatLong c3 = new LatLong(-90.0, -180.0);
System.out.println(c3.latitude == -90.0);
System.out.println(c3.longitude == -180.0);

// estas coordenadas são inválidas e devem lançar exceção
// faça serem rejeitadas e depois comente-as para não parar o programa
new LatLong(-91.0, 0.0);
new LatLong(100.0, 0.0);
new LatLong(10.0, -182.0);
new LatLong(10.0, 200.0);
new LatLong(-95.0, -200.0);

// imutabilidade: as linhas a seguir devem causar erro de compilação
// verifique se está de acordo e depois comente-as
LatLong c4 = new LatLong();
c4.latitude = 30.0;  // não deve permitir reatribuição
c4.longitude = 80.0; // não deve permitir reatribuição

// operações/comandos:
LatLong in = new LatLong(30.0, 50.0);
LatLong out = in.norte(5.0);
System.out.println(in.latitude == 30.0); // deve ser imutável
System.out.println(out.latitude == 35.0);
out.norte(5.0); // sem reatribuição sem alteração
System.out.println(out.latitude == 35.0);
out = out.norte(5.0); // reatribuindo
System.out.println(out.latitude == 40.0);
out = out.sul(60.0);
System.out.println(out.latitude == -20.0);
out = out.sul(30.0);
System.out.println(out.latitude == -50.0);
out = out.sul(-10.0);
System.out.println(out.latitude == -40.0);
out = out.norte(-10.0);
System.out.println(out.latitude == -50.0);
System.out.println(out.longitude == 50.0);
out = out.leste(50.0);
System.out.println(out.longitude == 100.0);
out = out.oeste(180.0);
System.out.println(out.longitude == -80.0);
out = out.oeste(-10.0);
System.out.println(out.longitude == -70.0);
out = out.leste(-10.0);
System.out.println(out.longitude == -80.0);

// consultas:
LatLong q = new LatLong();
System.out.println(q.latitude == 0);
System.out.println(q.longitude == 0);
System.out.println(q.noEquador() == true);
System.out.println(q.emGreenwich() == true);
q = q.norte(10.0);
System.out.println(q.latitude == 10);
System.out.println(q.noEquador() == false);
q = q.leste(10.0);
System.out.println(q.emGreenwich() == false);
q = q.leste(170.0);
System.out.println(q.longitude == 180.0);
System.out.println(q.emGreenwich() == false);
q = q.oeste(200.0);
System.out.println(q.longitude == -20.0);
System.out.println(q.emGreenwich() == false);
q = q.leste(5.0).leste(5.0).leste(5.0).leste(5.0);
System.out.println(q.longitude == 0.0);
System.out.println(q.emGreenwich() == true);

LatLong r = new LatLong(30.0, 70.0);
System.out.println(r.latitude == 30.0);
System.out.println(r.longitude == 70.0);
System.out.println(r.noNorte() == true);
System.out.println(r.noSul() == false);
System.out.println(r.noOriente() == true);
System.out.println(r.noOcidente() == false);
r = r.oeste(140.0).sul(60.0);
System.out.println(r.latitude == -30.0);
System.out.println(r.longitude == -70.0);
System.out.println(r.noNorte() == false);
System.out.println(r.noSul() == true);
System.out.println(r.noOriente() == false);
System.out.println(r.noOcidente() == true);
```

**Informações adicionais:**

- Imagem esclarecedora <http://eogn.com/images/newsletter/2014/Latitude-and-longitude.png>
- Sobre coordenadas decimais <http://opussig.blogspot.com.br/2013/01/coordenadas-geograficas-em-formato.html>



### Representações e Formatos de `LatLong`

Implementar os métodos de conversão para `LatLong` conforme os casos de teste.

```java
// As coordenadas entram em graus decimais com até quatro casas decimais
LatLong coord1 = new LatLong(12.5334, 77.2635);
System.out.println(coord1.toString()); // 12.5334, 77.2635
System.out.println(coord1.toString().equals("12.5334, 77.2635"));
// Não confunda o símbolo de grau ° com o de número ordinal º
System.out.println(coord1.toGrausDecimais()); // 12.5334°N, 77.2635°E
System.out.println(coord1.toGrausDecimais().equals("12.5334°N, 77.2635°E"));
// Em Graus, Minutos e Segundos com 3 decimais
System.out.println(coord1.toGrausMinutosSegundos()); // 12°32'0.240"N, 77°15'48.600"E
System.out.println(coord1.toGrausMinutosSegundos().equals("12°32'0.240\"N, 77°15'48.600\"E"));

LatLong coord2 = new LatLong(-75.9923, -52.1425);
System.out.println(coord2.toString()); // -75.9923, -52.1425
System.out.println(coord2.toString().equals("-75.9923, -52.1425"));
System.out.println(coord2.toGrausDecimais(); // 75.9923°S, 52.1425°W
System.out.println(coord2.toGrausDecimais().equals("75.9923°S, 52.1425°W"));
System.out.println(coord2.toGrausMinutosSegundos()); // 75°59'32.280"S, 52°8'33.000"W
System.out.println(coord2.toGrausMinutosSegundos().equals("75°59'32.280\"S, 52°8'33.000\"W"));

// LatLong coord3 = LatLong.fromString("63°18'12.300\"S, 76°56'43.120\"W"); // INCORRETO: PATCH
LatLong coord3 = LatLong.fromString("63°18'12.240\"S, 76°56'43.080\"W");
System.out.println(coord3.toString()); // -63.3034, -76.9453
System.out.println(coord3.toString().equals("-63.3034, -76.9453")); // -63.3034, -76.9453

LatLong coord4 = LatLong.fromString("-63.3034, -76.9453");
// System.out.println(coord4.toGrausMinutosSegundos().equals("63°18'12.300\"S, 76°56'43.120\"W")); // INCORRETO, PATCH:
System.out.println(coord4.toGrausMinutosSegundos().equals("63°18'12.240\"S, 76°56'43.080\"W"));
```

**Conversor de Coordenadas:** <http://maps.marnoto.com/conversor-coordenadas/> ou <https://www.sunearthtools.com/dp/tools/conversion.php?lang=pt>.



### **OPCIONAL** | Gerador de números pseudo-aleatórios

**Esta parte é opcional, se estás apertado de tempo, siga para a próxima**

Implementar um _Geradores de Números Pseudo-aleatórios_ ([PRNG](http://en.wikipedia.org/wiki/Pseudorandom_number_generator)). Os números são gerados por aritmética e, se usada a mesma semente, possuem sequência previsível.

O objetivo é criar um substituto para `Math.random()`, porém esta versão será Orientada a Objetos, em vez de usar um método estático como é feito em `Math`. Para obter um número aleatório é preciso instanciar o objeto `Randomizer` e invocar o método `next()`. **Não é para usar um randomizador do Java internamente**.

Como especificação, o gerador deve fornecer números do tipo `double` _n_ de modo que `0.0 =< n < 1.0`.

A implementação sugerida se chama [Middle Square Randomizer](http://pt-br.lmgtfy.com/?q=middle-square+method) e seus casos de teste estão a seguir. **Para passar deve imprimir nenhum `false`**.

```java
// como os testes não são precisos, precisamos gerar 1 milhão de vezes
// e checar se os números gerados estão no intervalo:
System.out.println("Testando se gera valores dentro do intervalo ...");
Randomizer randomizer1 = new Randomizer();
for (int i = 0; i < 1_000_000; i++) { // gerar 1 milhão de números
  double n = randomizer1.next();
  if ( ! (n >= 0 && n < 1.0)) {
    System.out.println("false: " + n + " fora do intervalo 0.0 =< n < 1.0");
  }
}
System.out.println();

System.out.println("Testando distribuição ...");
Randomizer randomizer2 = new Randomizer();
int[] variacao = new int[10];
for (int i = 0; i < 1_000_000; i++) { // gerar 1 milhão de números
  variacao[((int) (randomizer2.next() * 10))]++;
}
// deve ter +ou- 100.000 números em cada posição do array.
System.out.println(java.util.Arrays.toString(variacao));
System.out.println();

System.out.println("Testando nextInt ...");
Randomizer randomizer3 = new Randomizer();
for (int i = 0; i < 1_000_000; i++) {
  int n = randomizer3.nextInt(17); // deve ir de 0 a 17 inclusive!
  if ( ! (n >= 0 || n <= 17) {
    System.out.println("false: " + n + " fora do invervalo 0 =< n <= 17");
  }
}

for (int i = 0; i < 1_000_000; i++) {
  int n = randomizer3.nextInt(17, 51); // entre 17 e 51 inclusivo em ambas extremidades
  if ( ! (n >= 17 || n <= 51)) {
    System.out.println("false: " + n + " fora do invervalo 17 =< n <= 51");
  }
}
System.out.println();
```

**Help:**

Existem muitas técnicas para implementar PRNG's, como usar [Geradores Lineares Congruentes](https://www.google.com.br/search?q=geradores+lineares+congruentes) ou outros métodos que incluem o que iremos usar nesta atividade: [Middle Square Method](http://en.wikipedia.org/wiki/Middle-square_method).

O algoritmo é relativamente simples:

- Começamos com um número arbitrário como semente, por exemplo: `675248`,
- Obtemos o quadrado do número, neste caso: `455959861504`,
- Obtemos os seis dígitos centrais, neste caso: `959861`,
- Para obter um valor `0 =< n < 1` divide-se por `1000000`, o resultado é neste caso: `0.959861`,
- Repete-se a operação agora usando `959861` como semente.

Obs.: esse algoritmo pode ficar preso em _loop_ ou quebrar com execuções sucessivas, são necessárias algumas verificações e ajustes, por exemplo, assegurar-se que o _length_ do _square_ não diminua com o tempo (gambiarras aceitas).

**Adendo:** pode ser usado o `System.currentTimeMillis():long` para obter a semente, considere usar os 6 últimos números.

* * *

> There are known knowns. These are things we know that we know.
> There are known unknowns. That is to say, there are things that we know we don't know.
> But there are also unknown unknowns. There are things we don't know we don't know.
>
> -- **Donald Rumsfeld**

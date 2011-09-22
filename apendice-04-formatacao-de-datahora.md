## Formatação em templates

Formatação usada para template tag **{% now %}** e template filter **|date:""**.

<div class="tabela"><table>
<tr><th>Código</th><th>Descrição</th><th>Exemplo de resultado</th></tr>
<tr><td>a</td><td>'a.m.' ou 'p.m.'</td><td>'a.m.'</td></tr>
<tr><td>A</td><td>'AM' ou 'PM'.</td><td>'AM'</td></tr>
<tr><td>b</td><td>Mês, textual, 3 letras, minúsculas.</td><td>'jan'</td></tr>
<tr><td>d</td><td>Dia do mês, 2 dígitos com zero à esquerda quando menor que 10.</td><td>'01' to '31'</td></tr>
<tr><td>D</td><td>Dia do mês, textual, 3 letras.</td><td>'Seg'</td></tr>
<tr><td>f</td><td>Hora completa, formato de 12 horas, com minutos eliminados se for zero</td><td>'1', '1:30'</td></tr>
<tr><td>F</td><td>Mês, textual, longo</td><td>'Janeiro'</td></tr>
<tr><td>g</td><td>Hora, formato de 12 horas sem zero.</td><td>'1' a '12'</td></tr>
<tr><td>G</td><td>Hora, formato de 24 horas sem zero.</td><td>'0' a '23'</td></tr>
<tr><td>h</td><td>Hora, formato de 12 horas com zero</td><td>'01' a '12'</td></tr>
<tr><td>H</td><td>Hora, formato de 24 horas com zero</td><td>'00' a '23'</td></tr>
<tr><td>i</td><td>Minutos</td><td>'00' a '59'</td></tr>
<tr><td>j</td><td>Dia do mês sem zero</td><td>'1' to '31'</td></tr>
<tr><td>l</td><td>Dia da semana, textual, longo</td><td>'Segunda'</td></tr>
<tr><td>L</td><td>Valor booleano para indicar que ano é bissexto</td><td>True ou False</td></tr>
<tr><td>m</td><td>Mês, 2 dígitos com zero</td><td>'01' to '12'</td></tr>
<tr><td>M</td><td>Mês, textual, 3 letras</td><td>'Jan'</td></tr>
<tr><td>n</td><td>Mês sem zero</td><td>'1' to '12'</td></tr>
<tr><td>N</td><td>Mês abreviado no estilo Associated Press</td><td>'Jan.', 'Fev.', 'Março', 'Maio'</td></tr>
<tr><td>O</td><td>Diferença para horário Greenwich em número de horas.</td><td>'+0200'</td></tr>
<tr><td>P</td><td>Hora completa, tipo 12 horas, minutos e 'a.m.'/'p.m.', com minutos eliminados se for zero e tratamento de casos especiais 'meia-noite' e 'meio-dia'</td><td>'1 a.m.', '1:30 p.m.', 'meia-noite', 'meio-dia', '12:30 p.m.'</td></tr>
<tr><td>r</td><td>Data formatada no padrão RFC 2822.</td><td>'Thu, 21 Dec 2000 16:01:07 +0200'</td></tr>
<tr><td>s</td><td>Segundos, 2 dígitos com zero</td><td>'00' a '59'</td></tr>
<tr><td>S</td><td>Sufixo para números ordinários em inglês para o dia do mês, 2 caracteres</td><td>'st', 'nd', 'rd' or 'th'</td></tr>
<tr><td>t</td><td>Número total de dias que o mês daquela data possui</td><td>28 a 31</td></tr>
<tr><td>T</td><td>Fuso horário atual</td><td>'EST', 'MDT'</td></tr>
<tr><td>w</td><td>Dia da semana, dígitos sem zero</td><td>'0' (Domingo) a '6' (Sábado)</td></tr>
<tr><td>W</td><td>Número da semana no ano, padrão ISO-8601, com as semanas iniciando na Segunda</td><td>1, 53</td></tr>
<tr><td>y</td><td>Ano, 2 dígitos</td><td>'99'</td></tr>
<tr><td>Y</td><td>Ano, 4 dígitos</td><td>'1999'</td></tr>
<tr><td>z</td><td>Dia do ano</td><td>0 a 365</td></tr>
<tr><td>Z</td><td>Diferença de segundos do fuso horário. A diferença para fusos horários a oeste de UTC é sempre negativa, e para o leste de UTC é sempre positiva.</td><td>-43200 a 43200</td></tr>
</table></div>

Fonte: [http://docs.djangoproject.com/en/dev/ref/templates/builtins/#now](http://docs.djangoproject.com/en/dev/ref/templates/builtins/#now)

## Formatação no Python

Formatação usada para a função **datetime.strftime()**. Não traduz usando o sistema de internacionalização do Django, sempre trabalha com as configurações regionais do sistema operacional.

<div class="tabela"><table>
<tr><th>Código</th><th>Descrição</th><th>Exemplo de resultado</th></tr>
<tr><td>%a</td><td>Dia da semana, textual, 3 letras</td><td>Sun</td></tr>
<tr><td>%A</td><td>Dia da semana, textual, longo</td><td>Sunday</td></tr>
<tr><td>%b</td><td>Mês, textual, 3 letras</td><td>Nov</td></tr>
<tr><td>%B</td><td>Mês, textual, longo</td><td>November</td></tr>
<tr><td>%c</td><td>Data/hora completa</td><td>Sun Nov 23 18:44:17 2008</td></tr>
<tr><td>%d</td><td>Dia do mês, 2 dígitos, com zero à esquerda quando menor que 10</td><td>23</td></tr>
<tr><td>%H</td><td>Hora do dia, tipo 24 horas, com zero</td><td>09, 18</td></tr>
<tr><td>%I</td><td>Hora do dia, tipo 12 horas, com zero</td><td>09, 06</td></tr>
<tr><td>%j</td><td>Dia do ano</td><td>328</td></tr>
<tr><td>%m</td><td>Mês, 2 dígitos, com zero</td><td>11</td></tr>
<tr><td>%M</td><td>Minutos, 2 dígitos, com zero</td><td>01</td></tr>
<tr><td>%p</td><td>'AM' ou 'PM' para hora</td><td>&nbsp;</td></tr>
<tr><td>%S</td><td>Segundos, 2 dígitos, com zero</td><td>22</td></tr>
<tr><td>%U</td><td>Número da semana no ano, padrão ISO-8601, com as semanas iniciando no Domingo</td><td>47</td></tr>
<tr><td>%w</td><td>Dia da semana em dígito, com as semanas começando no Domingo e no dígito 0</td><td>0</td></tr>
<tr><td>%W</td><td>Número da semana no ano, padrão ISO-8601, com as semanas iniciando na Segunda</td><td>46</td></tr>
<tr><td>%x</td><td>Data no formato definido no sistema operacional</td><td>11/23/08</td></tr>
<tr><td>%X</td><td>Hora no formato definido no sistema operacional</td><td>19:01:22</td></tr>
<tr><td>%y</td><td>Ano, 2 dígitos, com zero</td><td>08</td></tr>
<tr><td>%Y</td><td>Ano, 4 dígitos, com zero</td><td>2008</td></tr>
<tr><td>%Z</td><td>Fuso horário (vazio se não estiver definido)</td><td>UTC</td></tr>
<tr><td>%%</td><td>Caracter '%'</td><td>&nbsp;</td></tr>
</table></div>

Fonte: [http://www.python.org/doc/2.5.2/lib/module-time.html](http://www.python.org/doc/2.5.2/lib/module-time.html)


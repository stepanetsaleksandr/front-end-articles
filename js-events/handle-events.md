<h2>Обработка событий</h2>

<p>Любое действие пользователя на странице называется "событие". Есть большой <a rel="noopener noreferrer" target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/Events">список</a> событий. Наиболее используемые - это <code>'click'</code>, <code>'change'</code> и т.д. Любое действие ('событие') совершается над каким-то конкретным элементом. То есть в этом процессе фигурируют две вещи: 
  <ol>
    <li>DOM элемент, на который совершено событие</li>
    <li>Какое именно событие совершено (<code>'click'</code>, <code>'change'</code> ...)</li>
  </ol>    
</p>

<p>Чтобы в нашей программе как-то реагировать на действие пользователя - эти события нужно обработать. Обработать событие означает запустить какой-то код в ответ на событие. Например отправить заказ на сервер, когда пользователь нажмет на кнопку.</p>

<p>Другими словами, нам нужен DOM элемент, на котором будет совершено событие, какое именно событие будем обрабатывать и код, который запустится после наступления события на заданном элементе</p>

<p>В JavaScript обработчик события можно задать 3-мя способами:
  <ul>
    <li>💩 <code>&lt;button onclick="alert('clicked')"&gt;send&lt;/button&gt;</code> - назначить обработчик прямо в HTML разметке. Это наихудший способ, так как мы пишем JavaScript прямо в HTML. Да и написать туда несколько команд не так уж просто =)</li>
    <li>💩 Назначить обработчик, как свойство DOM элемента
      <pre>
        &lt;!-- html --&gt;
        &lt;button class="send-btn"&gt;send&lt;/button&gt;
      </pre>
      <pre>
        /* js */
        const buttonElem = document.querySelector('.send-btn');
        buttonElem.onClick = function() {
          alert('clicked');
        }
      </pre>
      Нужно найти DOM элемент и присвоить ему в нужное свойство ф-цию, которая выполнится, когда на этом элементе сработает событие, что соответствует названию свойства. Свойства <code>onClick</code>, <code>onChange</code>, <code>onMouseEnter</code> и т.д. Имя свойства состоит из префикса 'on', а дальше в camelCase идет название события 
    </li>
    <li>😎Назначить обработчик с помощью ф-ции <code>element.addEventListener</code>
      <pre>
        &lt;!-- html --&gt;
        &lt;button class="send-btn"&gt;send&lt;/button&gt;
      </pre>
      <pre>
        /* js */
        const buttonElem = document.querySelector('.send-btn');
        function handleClick() {
          alert('clicked');
        }
        buttonElem.addEventListener('click', handleClick);
      </pre>
      У DOM элемента есть метод <code>addEventListener</code>. Метод принимает ТРИ!!! аргумента:
      <ol>
        <li>название события</li>
        <li>ф-ция</li>
        <li>доп. настройки</li>
      </ol>
    </li>
  </ul>

  Лучший из 3-х перечисленных выше способов - это <code>addEventListener</code>
</p>


<h2><code>addEventListener</code></h2>

<p>
  Особенности <code>addEventListener</code>
  <ul>
    <li>Первым аргументом идет строка - название события</li>
    <li>Вторым аргументом нужно передать ссылку на ф-цию, которая объявлена отдельно. Нельзя объявлять ф-цию прямо там же
      <pre>
        /* 👍 правильно */
        const buttonElem = document.querySelector('.send-btn');
        function handleClick() {
          alert('clicked');
        }
        buttonElem.addEventListener('click', handleClick);
      </pre>
      <pre>
        /* 👎 не правильно */
        const buttonElem = document.querySelector('.send-btn');
        buttonElem.addEventListener('click', function () {
          alert('clicked');
        });
      </pre>
      Работать будет, но есть нюансы (с отменой обработчика)
    </li>
    <li>Обработчик, назначенный с помощью <code>addEventListener</code> можно отменить с помощью <code>removeEventListener</code>
      <pre>
        const buttonElem = document.querySelector('.send-btn');
        function handleClick() {
          alert('clicked');
        }
        buttonElem.addEventListener('click', handleClick);
        // ... где-то дальше по коду
        const buttonElem = document.querySelector('.send-btn');
        buttonElem.removeEventListener('click', handleClick);
      </pre>
      Чтобы отменить обработчик, нужно вызвать <code>removeEventListener</code> с теми же аргументами, что и <code>addEventListener</code> - строка события в первом аргументе, ссылка на ту же ф-цию во втором. Обратите внимание, что если бы мы написали ф-цию прямо в <code>addEventListener</code>, то было бы не возможно передать ту же ф-цию в <code>removeEventListener</code> и не получилось бы отменить обработчик. После вызова <code>removeEventListener</code>, ф-ция <code>handleClick</code> больше не будет запускаться при клике на кнопку
    </li>
  </ul>
</p>

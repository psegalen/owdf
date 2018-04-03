# One-Way Data Flow

Pierre Segalen

@fa[twitter gp-contact](@psegalen)

---?image=images/OneWayDataFlow.png&size=auto 80%

---?image=images/OneWayDataFlow_render.png&size=auto 80%

---

### React

* Composants
* Etat local
* JSX (balises dans JS)
* DOM virtuel

---?image=images/VirtualDOM.png&size=auto 70%

<span style="font-style: italic; font-size:25px">src: https://redux.js.org</span>

---

### React

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  componentDidMount() {
    this.timerID = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(<Clock />, document.getElementById("root"));
```

@[31](Rendu en JSX d'un composant "Clock".)
@[1](Déclaration du composant.)
@[2-5](Initialisation de l'état local.)
@[7-9](Cycle de vie, exécuté après le premier rendu.)
@[11-13](Cycle de vie, exécuté avant la suppression de l'instance.)
@[15-19](Appelé dans le setInterval déclaré dans componentDidMount.)
@[21-28](Rendu du composant.)

---

### Clock

[Démo](https://codepen.io/gaearon/pen/amqdNA?editors=0010)

---?image=images/OneWayDataFlow_state.png&size=auto 80%

---

### Redux

* Etat applicatif
* Découpé en sous-états appelés "Reducers"
* Reducer = fonction pure
  * `(store, action) => store`
* Données immutables

---?image=http://jonnyreeves.co.uk/images/2016/redux-middleware/redux-with-middleware.png&size=70% auto

<span style="font-style: italic; font-size:25px">src: http://jonnyreeves.co.uk/2016/redux-middleware/</span>

---

### Todo list

[Démo](http://localhost:3000)

---?image=images/OneWayDataFlow_actions.png&size=auto 80%

---

### Middlewares Redux

* Logs
* Statistiques
* Exécution des actions (side effects management):
  * `redux-promise`
  * `redux-thunk`
  * `redux-saga`
  * `redux-observable`
  * `...`

---

### `redux-observable`

* Basé sur RxJS
* Concept central : Epics

```javascript
const pingEpic = action$ =>
  action$
    .ofType(PING)
    .delay(1000) // Asynchronously wait 1000ms then continue
    .mapTo({ type: PONG });
```

[Démo](http://jsbin.com/jexomi/edit?js,output)

---

### Pourquoi ?

* Modularité
  * Maintenabilité
  * Evolutivité
  * Testabilité
* Flexibilité
  * 100% agile
* ❤️ `react-native`

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

---?image=images/VirtualDOM.png&size=auto 80%

<span style="font-style: italic; color:#333; font-size:30px">src: https://redux.js.org</span>

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

---?image=http://jonnyreeves.co.uk/images/2016/redux-middleware/redux-with-middleware.png&size=80% auto

<span style="font-style: italic; color:#333; font-size:30px">src: http://jonnyreeves.co.uk/2016/redux-middleware/</span>

---

### Todo list

[Démo](http://localhost:3000)

---?image=images/OneWayDataFlow_actions.png&size=auto 80%

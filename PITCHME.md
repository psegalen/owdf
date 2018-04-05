# One-Way Data Flow

Pierre Segalen

@fa[twitter gp-contact](@psegalen)

---?image=images/OneWayDataFlow.png&size=auto 80%

---?image=images/OneWayDataFlow_render.png&size=auto 80%

---

### React

* Composants
* JSX (balises dans le JS)
* DOM virtuel
* Etat local / propriétés = immutable

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
```

@[2-5](Initialisation de l'état local)
@[7-13](Cycle de vie de l'instance du composant)
@[15-19](Appelé dans le setInterval déclaré dans componentDidMount)
@[21-28](Rendu du composant)

---

### Clock

[Démo](https://codepen.io/psegalen/pen/GxYRqm?editors=0010)

---

### ⚠️ Synchronisation des états

Plusieurs composants peuvent partager les mêmes données

Exemple, site de e-commerce :

* Bouton d'ajout au panier
* Bouton Panier dans le header
* Page Panier

N'utiliser que les états locaux = nécessité de les synchroniser

---?image=images/OneWayDataFlow_state.png&size=auto 80%

---

### Redux

* Etat applicatif
* Découpé en sous-états gérés par des "Reducers"
* Reducer = fonction pure
  * `(store, action) => store`
* Données immutables

---?image=http://jonnyreeves.co.uk/images/2016/redux-middleware/redux-with-middleware.png&size=70% auto

<span style="font-style: italic; font-size:25px">src: http://jonnyreeves.co.uk/2016/redux-middleware/</span>

---

### Un reducer

```javascript
const todos = (state = [], action) => {
  switch (action.type) {
    case "ADD_TODO":
      return [
        ...state,
        {
          id: action.id,
          text: action.text,
          completed: false
        }
      ];
    case "TOGGLE_TODO":
      return state.map(
        todo =>
          todo.id === action.id ? { ...todo, completed: !todo.completed } : todo
      );
    default:
      return state;
  }
};
```

@[1](Signature de tout reducer)
@[2](La logique du reducer dépend du type d'action)
@[3-16](Les données sont toujours immutables)
@[17-18](Toutes les actions passent dans tous les reducers => il faut toujours renvoyer le state inchangé quand le reducer ne gère pas l'action)

---

### Counter

[Démo](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/counter)

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

---?image=images/OneWayDataFlow_actions.png&size=auto 80%

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

### Un problème récurrent : annuler une requête

```javascript
const fetchUserEpic = action$ =>
  action$.ofType(FETCH_USER).mergeMap(action =>
    ajax
      .getJSON(`/api/users/${action.payload}`)
      .map(response => fetchUserFulfilled(response))
      .takeUntil(action$.ofType(FETCH_USER_CANCELLED))
  );
```

---

### Pourquoi ?

* Modularité
  * Maintenabilité
  * Testabilité
* Flexibilité
  * 100% agile
* ❤️ `react-native`

---?image=images/OneWayDataFlow_questions.png&size=auto 60%

### Questions

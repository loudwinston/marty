---
layout: page
title: Queries API
id: api-queries
section: Queries
---
{% sample %}
classic
=======
var UserQueries = Marty.createQueries({
  id: 'UserQueries',
  getUser: function (id) {
    this.dispatch(UserActions.RECEIVE_USER_STARTING, id);
    UserAPI.getUser(id).then(function (res) {
      if (res.status === 200) {
        this.dispatch(UserActions.RECEIVE_USER, res.body, id);
      } else {
        this.dispatch(UserActions.RECEIVE_USER_FAILED, id);
      }
    }.bind(this)).catch(function (err) {
      this.dispatch(UserActions.RECEIVE_USER_FAILED, id, err);
    }.bind(this))
  }
});

es6
===
class UserQueries extends Marty.Queries {
  getUser(id) {
    this.dispatch(UserActions.RECEIVE_USER_STARTING, id);
    UserAPI.getUser(id).then((res) => {
      if (res.status === 200) {
        this.dispatch(UserActions.RECEIVE_USER, res.body, id);
      } else {
        this.dispatch(UserActions.RECEIVE_USER_FAILED, id);
      }
    }).catch((err) => this.dispatch(UserActions.RECEIVE_USER_FAILED, id, err));
  }
}
{% endsample %}

<h2 id="id">id</h2>

A unique identifier (*required*). Needed by the [registry]({% url /api/registry/index.html %}) to uniquely identify the type.

<h2 id="displayName">displayName</h2>

An (optional) display name for the action creator. Used for richer debugging. We will use the Id if displayName hasn't been set. If you're using ES6 classes, displayName will automatically be the name of the class.

<h2 id="dispatch">dispatch(type, [...])</h2>

Dispatches an action payload with the given type. Any [action handlers]({% url /api/stores/index.html#handleAction %}) will be invoked with the given action handlers.

<h2 id="for">for(obj)</h2>

Resolves the instance of the object for the objects Marty context. The context can either be the object itself or available at ``obj.context`` or ``obj.context.marty``.
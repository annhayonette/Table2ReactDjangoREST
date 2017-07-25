# Documentation:

## libraries required:

- "react": "15.5.4",
- "moment": "2.18.0" <https://momentjs.com/>,

## Iniciar Tabla2

```javascript

const service = 'https://host.com/api/';
const query = '&organization_id=1';

function getService() {
    const params = '';
    return {
        api: service + '?',
        query: query,
    };
}

<table2
    service={getService()}
    data={data}
    options={options}>
</table2>
```

- atributo `service`: Si se utiliza un filtro desde afuera de `Table2` se debe pasar al atributo del componente `service` un objeto con la base de la `api` y el `query` inicial.

  - Ejemplo:

    - `service` = <https://host.com/api/>
    - `query` = `&organization_id=1`

- atributo `data`: aqui se debe pasar el objeto que retorna el servicio (este objeto es el que genera Django REST Framework cuando se utiliza la clase `PaginationDjangoRestTable` incluida en este repositorio)

  - Ejemplo:

```json

{
    "next": null,
    "previous": null,
    "count": 8,
    "limit": 10,
    "current": 1,
    "final": 1,
    "results": [
        {
            "id": 55,
            "organization_id": 1,
            "rut": 116354756,
            "name": "Jhon",
            "last_names": "Carpenter",
            "phone": "+56954900747",
            "emergency_phone": "+56954900747",
            "job_title": "Dev",
        },
    ]
}
```

- atributo `options`: este el el objeto principal desde donde se define la tabla

```javascript
const options = {
    className: 'table table-bordered',
    actions: {
        view: { Component: EditPerson, cssClass: 'btn btn-primary' },
        css: { whiteSpace: 'nowrap', minWidth: '182px' },
    },
    hiddenActions: !isCoordinator(),
    columns: [
        {
            title: 'RUT',
            name: 'rut',
            textIsEmpty: 'rut',
            templateWithInstance: ComponentColumn,
            inputSeachComponet: ComponentInputSearch,
            inputSearchPlaceholder: 'Ingrese Teléfono',
            inputSeach: 'icontains',
            noSorting: true,
            css: { minWidth: '50px' },
        },
    ],
};
```

nombre                 | valor                              | descripcción
---------------------- | ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
className              | 'table table-hover table-bordered' | un `string` con las clases que se quiera agregar a la tabla
actions                | object                             | objeto con la definición de las acciones que mostrara en la columna acciones
actions.view           | object                             | espera un `Component` para ser renderizado en cada `row` y clases css
actions.view.Component | `React.Component`                  | espera un componente `React`, en el componente es accesible como propiedad el objeto de la `row` donde este se creara y las clases css. `(ver codigo de ejemplo 01)`
actions.view.cssClass  | string                             | clases css dentro de un string
actions.css            | string                             | string que sera pasado directamente a la etiqueta `<td>` de la columna acciones
hiddenActions          | boolean                            | posibilita oculat o mostrar la acciones, por defecto es `true`
columns                | array                              | esta seccion es donde se mapea los atributos de los objetos que retorna el servicio, el orden de las columnas tiene directa relación con el orden de los objetos en este `array`

### array columns

nombre               | valor                                    | descripcción
-------------------- | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
title                | string                                   | titulo de la columna que se ve en el header
name                 | string                                   | nombre del atributo del objeto que se desea mostrar , existe la posibilidad de motrar atributos anidados de la siguiente manera `roles.admin.id`
textIsEmpty          | string                                   | texto que se mostrara si el valor del atributo esta vacio
templateWithInstance | function: params instance, definitionCol | por defecto cada columna muestra el valor mapeado dentro del `<td>`, pero si se desea un comportamiento especifico, se puede pasar una funcion con ese contenido `(ver codigo de ejemplo 02)`, la funcion utilizada siempre tendra dos parametros que le pasa `Table2`: `instance`(la instancia del objeto actual), `definitionCol`(el array columns), por defecto no es necesario utilizar este atributo
inputSeachComponet   | function: params handlerChange, element  | funcion para sobreescribir el input por defecto para los filtros, se pasa desde `Table2`: `handlerChange`(callback para ejecutar el onChange y enviar el value del input al servicio), `element`(objeto actual de la columna que se define en el array columns)

handlerChange: this.onChangeSearch, element: props.element,

# FIELD: columns

@name: attr.resolveIdToText @type: Object @example: { '1' : { label: 'energi', value: '1' }, ...

# }

@name: attr.mapChildren @type: Object @example: { ernc_factor: { name: 'ernc factor', type: 'danger', }, ...

```

},
```

--------------------------------------------------------------------------------

@name: attr.replaceBooleanValue @type: Object @example: { isTrue: 'OK', isFalse: 'Fail',

```

},
```

--------------------------------------------------------------------------------

## Códigos de Ejemplo

- 01 `Componente React para actions.view.Component`

```javascript
import React, { Component } from 'react';
import PropTypes from 'prop-types';
import { Link } from 'react-router-dom';

class ComponentActionsView extends Component {
    render() {
        return (
          <div className="btn-group">
            <Link
              to={`/url-01/${this.props.object.id}`}
              className="btn btn-danger btn-sm"
            >Delete</Link>
            <Lin
              to={`/url-02/${this.props.object.id}`}
              className={this.props.cssClass}
            >Edit</Link>
          </div>
        );
    }
}
ComponentActionsView.propTypes = {
    cssClass: PropTypes.string.isRequired,
    object: PropTypes.object.isRequired,
};

export default ComponentActionsView;
```

- 02 `functions` para cambiar el componente por defecto que se reenderiza en las columnas

```javascript
function RUT(instance) {
    return ( <p>{rutFormat(rutAddDv(instance.rut))}</p>);
}
function User(instance) {
    return (<p>{instance.user.is_active ? 'active' : 'inactive'</p>);
}
function Phone(instance) {
    return (<p>{formatPhone(instance.phone)}</p>);
}
```

@name: attr.title: @type: String

# @example: 'The Title'

@name: attr.name @type: String

# @example: 'name_column_service'

@name: attr.textIsEmpty @type: String

# @example: 'The field is Empty, Sorry!!!'

# @name: attr.childrenTooltip: true TODO

@name: attr.css @type: Object @example: { whiteSpace: 'nowrap', color: 'red', font-size: '15px', ...

```

},
```

--------------------------------------------------------------------------------

@name: attr.cssClass @type: String @example: 'btn btn-primary bg-color-active'

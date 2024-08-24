## Configurações inciais
[Tutorial](https://www.youtube.com/watch?v=7b42lVMdEjE)

1. Criar um projeto do zero: `npm create vite@latest`
1. Instalar as dependências: `npm i react-router-dom`

Adicionar no `main.jsx` a seguinte modificação:

### Primeira forma de criação de rotas:

```jsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

const router = createBrowserRouter([
    {
        path: "/",
        element: <Component1 />
    },
    {
        path: "/",
        element: <Component2 />
    }
]);

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <RouterProvider router={router} >
  </StrictMode>,
```

### Segunda forma de criação de rotas:

```jsx
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import HomePage from './view/Home/HomePage'
import LoginPage from './view/Login/LoginPage'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/login" element={<LoginPage />} />
      </Routes>
    </BrowserRouter>
  )
}

export default App
```

## Transformando em Lazy

Basta utilizar o método `const Componente = React.lazy(() => import('./...')))` dentro do componente `<Suspense fallback={<ComponentDeLoading />}><Componente /></Suspense>` que será exibido o componente de loading até que o componente seja completamente carregado.

## Rotas com Lazy

Basta envelopar o componente roteado (element) com um `Suspense`;

## Rotas com guards
[Tutorial](https://www.youtube.com/watch?v=2k8NleFjG7I)

Criação do método de garantia de autenticação:

```jsx
import { Outlet, Navigate } from 'react-router-dom'

const PrivateRoutes = () => {
    let auth = {'token':false}
    return(
        auth.token ? <Outlet/> : <Navigate to="/login"/>
    )
}

export default PrivateRoutes
```

Rotear as rotas privadas como filhas de um router do componente criado acima:

```jsx
            <Route element={<PrivateRoutes />}>
                <Route element={<Home/>} path="/" exact/>
                <Route element={<Products/>} path="/products"/>
            </Route>
```
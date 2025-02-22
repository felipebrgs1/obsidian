# 📌 Layout Padrão no React com React Router

## 🔹 O que é um Layout Padrão?

Um **layout padrão** em React é um componente que define a estrutura fixa da aplicação, como **sidebar, navbar, rodapé**, enquanto o conteúdo muda dinamicamente conforme a rota acessada.

No **React Router**, isso é feito utilizando o componente `<Outlet />`, que serve como um espaço reservado onde as páginas são renderizadas.

---

## 🔹 Exemplo de Implementação

### 1️⃣ Criando o Layout (Page.tsx)

```tsx
import React from "react";
import { Outlet } from "react-router-dom";
import { AppSidebar } from "@/components/app-sidebar";
import { Breadcrumb, BreadcrumbItem, BreadcrumbLink, BreadcrumbList, BreadcrumbPage, BreadcrumbSeparator } from "@/components/ui/breadcrumb";
import { Separator } from "@/components/ui/separator";
import { SidebarInset, SidebarProvider, SidebarTrigger } from "@/components/ui/sidebar";

export default function Page() {
  return (
    <SidebarProvider>
      <AppSidebar />
      <SidebarInset>
        <header className="flex h-16 items-center gap-2">
          <div className="flex items-center gap-2 px-4">
            <SidebarTrigger className="-ml-1" />
            <Separator orientation="vertical" className="mr-2 h-4" />
            <Breadcrumb>
              <BreadcrumbList>
                <BreadcrumbItem className="hidden md:block">
                  <BreadcrumbLink href="#">Home</BreadcrumbLink>
                </BreadcrumbItem>
                <BreadcrumbSeparator className="hidden md:block" />
                <BreadcrumbItem>
                  <BreadcrumbPage>Página Atual</BreadcrumbPage>
                </BreadcrumbItem>
              </BreadcrumbList>
            </Breadcrumb>
          </div>
        </header>
        
        {/* Aqui as páginas serão injetadas */}
        <main className="flex-1 p-4">
          <Outlet />
        </main>
      </SidebarInset>
    </SidebarProvider>
  );
}
```

---

### 2️⃣ Configurando as Rotas (router.tsx)

```tsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Page from "./app/dashboard/page";
import Home from "./pages/Home";
import About from "./pages/About";

export default function App() {
  return (
    <Router>
      <Routes>
        {/* Definindo o layout padrão */}
        <Route path="/" element={<Page />}>
          <Route index element={<Home />} /> {/* Página inicial */}
          <Route path="about" element={<About />} /> {/* Página "Sobre" */}
        </Route>
      </Routes>
    </Router>
  );
}
```

---

## 🔹 Explicação

- O `Page.tsx` contém a **sidebar, header e estrutura fixa**.
- O `<Outlet />` dentro do `Page.tsx` **renderiza dinamicamente** as páginas dentro do layout.
- No arquivo de rotas:
    - `<Route path="/" element={<Page />}>` define o layout como o **componente pai**.
    - `<Route index element={<Home />} />` carrega `Home` na URL `/`.
    - `<Route path="about" element={<About />} />` carrega `About` na URL `/about`.

Com isso, o layout se mantém fixo enquanto as páginas mudam! 🚀

Para adicionar um **layout extra** no React Router, você pode criar um novo componente de layout e definir rotas específicas para ele.

## 🔹 Como adicionar um Layout Extra?

Se, por exemplo, algumas páginas precisarem de um layout **diferente** (como uma página de administração sem sidebar), você pode fazer assim:

---

### 1️⃣ Criando um Novo Layout (AdminLayout.tsx)

```tsx
import React from "react";
import { Outlet } from "react-router-dom";

export default function AdminLayout() {
  return (
    <div className="flex flex-col min-h-screen">
      <header className="h-16 bg-gray-800 text-white flex items-center px-4">
        <h1>Admin Panel</h1>
      </header>
      <main className="flex-1 p-4">
        <Outlet />
      </main>
    </div>
  );
}
```

---

### 2️⃣ Configurando as Rotas para o Novo Layout

Agora, ajustamos o `router.tsx` para incluir o novo layout:

```tsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Page from "./app/dashboard/page";
import AdminLayout from "./app/admin/AdminLayout";
import Home from "./pages/Home";
import About from "./pages/About";
import AdminDashboard from "./pages/AdminDashboard";
import AdminSettings from "./pages/AdminSettings";

export default function App() {
  return (
    <Router>
      <Routes>
        {/* Layout Padrão */}
        <Route path="/" element={<Page />}>
          <Route index element={<Home />} />
          <Route path="about" element={<About />} />
        </Route>

        {/* Layout Administrativo */}
        <Route path="/admin" element={<AdminLayout />}>
          <Route index element={<AdminDashboard />} />
          <Route path="settings" element={<AdminSettings />} />
        </Route>
      </Routes>
    </Router>
  );
}
```

---

## 🔹 Explicação

- Criamos o `AdminLayout.tsx` para as páginas de administração.
- O `<Outlet />` no `AdminLayout` permite que as páginas de `/admin/*` sejam renderizadas dentro dele.
- No `router.tsx`, definimos que:
    - O layout `Page` é usado para `/` e `/about`.
    - O layout `AdminLayout` é usado para `/admin` e suas rotas filhas (`/admin/settings`).

Agora, qualquer página dentro de `/admin` usará o layout sem sidebar! 🚀


### 🔹 Por que essa abordagem é usada em produção?

1. **Melhor organização do código**
    
    - Separa a estrutura fixa (navbar, sidebar, breadcrumbs, etc.) do conteúdo dinâmico das páginas.
2. **Eficiência na renderização**
    
    - O layout não precisa ser recriado a cada mudança de rota, apenas o conteúdo interno é atualizado.
3. **Flexibilidade para múltiplos layouts**
    
    - Aplicações grandes geralmente têm layouts diferentes para **usuários comuns, administradores, áreas autenticadas**, etc.
4. **Escalabilidade**
    
    - Permite adicionar novos layouts sem refatorar todo o sistema.

### 🔹 Exemplo de produção com múltiplos layouts:

```tsx
<Routes>
  {/* Layout principal */}
  <Route path="/" element={<MainLayout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
  </Route>

  {/* Layout de administração */}
  <Route path="/admin" element={<AdminLayout />}>
    <Route index element={<AdminDashboard />} />
    <Route path="settings" element={<AdminSettings />} />
  </Route>

  {/* Layout de autenticação */}
  <Route path="/auth" element={<AuthLayout />}>
    <Route path="login" element={<Login />} />
    <Route path="register" element={<Register />} />
  </Route>
</Routes>
```
 🚀
# faz-hoje
Crie um SaaS web chamado "FazHoje".




Estilo visual:
- Fundo preto (#0B0B0B)
- Verde neon (#2BFF00) para botões de ação
- Vermelho (#FF2E2E) para falhas
- Design minimalista, moderno, brutalista leve
- Tipografia grande, foco em números e ações




Funcionalidades:
- Criar tarefa com:
  - Nome
  - Prazo (data)
  - Valor da penalidade (R$)
- Botão "Concluí"
- Se passar do prazo sem concluir:
  - Marcar como falha
  - Destacar em vermelho
- Dashboard simples
- Ranking de usuários (quem mais concluiu)




Sem recursos avançados.
Foco em simplicidade, pressão psicológica e ação.

export default function App() {
  return (
    <div className="min-h-screen bg-[#0B0B0B] text-white font-sans">
      <header className="p-6 border-b border-[#1f1f1f]">
        <h1 className="text-2xl font-bold text-[#2BFF00]">FazHoje</h1>
        <p className="text-sm text-gray-400">
          Faça. Ou pague.
        </p>
      </header>

      <main className="p-6 max-w-xl mx-auto">
        <div className="bg-[#121212] p-6 rounded-xl mb-6">
          <h2 className="text-lg font-semibold mb-4">
            Criar nova tarefa
          </h2>

          <input
            placeholder="O que você precisa fazer?"
            className="w-full mb-3 p-3 rounded bg-[#0B0B0B] border border-[#1f1f1f]"
          />

          <input
            type="date"
            className="w-full mb-3 p-3 rounded bg-[#0B0B0B] border border-[#1f1f1f]"
          />

          <input
            type="number"
            placeholder="Penalidade (R$)"
            className="w-full mb-4 p-3 rounded bg-[#0B0B0B] border border-[#1f1f1f]"
          />

          <button className="w-full bg-[#2BFF00] text-black font-bold py-3 rounded hover:opacity-90">
            Criar tarefa
          </button>
        </div>

        <div className="bg-[#121212] p-6 rounded-xl">
          <h2 className="text-lg font-semibold mb-4">
            Suas tarefas
          </h2>

          <div className="flex items-center justify-between p-4 rounded bg-[#0B0B0B] border border-[#1f1f1f]">
            <div>
              <p className="font-medium">Estudar 30 minutos</p>
              <p className="text-sm text-gray-400">
                Penalidade: R$10
              </p>
            </div>

            <button className="bg-[#2BFF00] text-black px-4 py-2 rounded font-bold">
              Concluí
            </button>
          </div>
        </div>
      </main>
    </div>
  );
}

Se data_atual > prazo && status != concluído:
  status = falhou
  aplicar penalidade
  marcar em vermelho no dashboard

User
- id
- email
- password
- created_at

Task
- id
- user_id
- title
- deadline
- penalty_value
- status (pending | done | failed)
- created_at

Implemente autenticação de usuários com:
- Cadastro por email e senha
- Login
- Proteção de rotas (dashboard só para logados)

A cada acesso ao dashboard:
  Para cada tarefa do usuário:
    Se status == "pending" e data_atual > deadline:
      status = "failed"

{tasks.map(task => (
  <div
    key={task.id}
    className={`p-4 rounded mb-3 border ${
      task.status === "failed"
        ? "border-[#FF2E2E] bg-[#1a0000]"
        : task.status === "done"
        ? "border-[#2BFF00] bg-[#001a00]"
        : "border-[#1f1f1f] bg-[#0B0B0B]"
    }`}
  >
    <h3 className="font-semibold">{task.title}</h3>
    <p className="text-sm text-gray-400">
      Penalidade: R${task.penalty_value}
    </p>

    {task.status === "pending" && (
      <button
        onClick={() => markAsDone(task.id)}
        className="mt-3 bg-[#2BFF00] text-black px-4 py-2 rounded font-bold"
      >
        Concluí
      </button>
    )}

    {task.status === "failed" && (
      <p className="mt-3 text-[#FF2E2E] font-bold">
        Falhou ❌
      </p>
    )}
  </div>
))}

Implemente plano de assinatura mensal no valor de R$29
com bloqueio do app para não assinantes.

Ranking:
- Usuários ordenados por:
  (tarefas concluídas - tarefas falhadas)

module.exports = {
  content: ["./src/**/*.{js,jsx}"],
  theme: {
    extend: {
      colors: {
        bg: "#0B0B0B",
        card: "#121212",
        green: "#2BFF00",
        red: "#FF2E2E",
        border: "#1f1f1f",
        muted: "#A1A1A1",
      },
    },
  },
  plugins: [],
}

import { createClient } from "@supabase/supabase-js";

export const supabase = createClient(
  "https://SEU_PROJETO.supabase.co",
  "SUA_PUBLIC_ANON_KEY"
);

create table tasks (
  id uuid default uuid_generate_v4() primary key,
  user_id uuid references auth.users on delete cascade,
  title text not null,
  deadline timestamp not null,
  penalty numeric not null,
  status text default 'pending',
  created_at timestamp default now()
);


import { useState } from "react";
import { supabase } from "./supabase";

export default function Auth() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const signIn = async () => {
    await supabase.auth.signInWithPassword({ email, password });
  };

  const signUp = async () => {
    await supabase.auth.signUp({ email, password });
  };

  return (
    <div className="min-h-screen bg-bg flex items-center justify-center text-white">
      <div className="bg-card p-8 rounded w-80">
        <h1 className="text-green text-2xl font-bold mb-6">FazHoje</h1>

        <input
          placeholder="Email"
          onChange={(e) => setEmail(e.target.value)}
          className="w-full mb-3 p-3 bg-bg border border-border rounded"
        />

        <input
          type="password"
          placeholder="Senha"
          onChange={(e) => setPassword(e.target.value)}
          className="w-full mb-4 p-3 bg-bg border border-border rounded"
        />

        <button onClick={signIn} className="w-full bg-green text-black font-bold py-2 rounded mb-2">
          Entrar
        </button>

        <button onClick={signUp} className="w-full border border-green py-2 rounded">
          Criar conta
        </button>
      </div>
    </div>
  );
}

import { useState } from "react";
import { supabase } from "./supabase";

export default function CreateTask({ user, reload }) {
  const [title, setTitle] = useState("");
  const [deadline, setDeadline] = useState("");
  const [penalty, setPenalty] = useState("");

  const createTask = async () => {
    await supabase.from("tasks").insert([
      {
        title,
        deadline,
        penalty,
        user_id: user.id,
      },
    ]);

    reload();
  };

  return (
    <div className="bg-card p-6 rounded mb-6">
      <h2 className="mb-4 font-semibold">Nova tarefa</h2>

      <input
        placeholder="O que você precisa fazer?"
        onChange={(e) => setTitle(e.target.value)}
        className="w-full mb-3 p-3 bg-bg border border-border rounded"
      />

      <input
        type="datetime-local"
        onChange={(e) => setDeadline(e.target.value)}
        className="w-full mb-3 p-3 bg-bg border border-border rounded"
      />

      <input
        type="number"
        placeholder="Penalidade (R$)"
        onChange={(e) => setPenalty(e.target.value)}
        className="w-full mb-4 p-3 bg-bg border border-border rounded"
      />

      <button onClick={createTask} className="w-full bg-green text-black font-bold py-2 rounded">
        Criar tarefa
      </button>
    </div>
  );
}

import { supabase } from "./supabase";

export default function TaskList({ tasks, reload }) {
  const markDone = async (id) => {
    await supabase.from("tasks").update({ status: "done" }).eq("id", id);
    reload();
  };

  return (
    <>
      {tasks.map((task) => (
        <div
          key={task.id}
          className={`p-4 rounded mb-3 border ${
            task.status === "failed"
              ? "border-red bg-[#1a0000]"
              : task.status === "done"
              ? "border-green bg-[#001a00]"
              : "border-border bg-bg"
          }`}
        >
          <p className="font-semibold">{task.title}</p>
          <p className="text-sm text-muted">R${task.penalty}</p>

          {task.status === "pending" && (
            <button
              onClick={() => markDone(task.id)}
              className="mt-3 bg-green text-black px-4 py-2 rounded font-bold"
            >
              Concluí
            </button>
          )}

          {task.status === "failed" && (
            <p className="mt-3 text-red font-bold">Falhou ❌</p>
          )}
        </div>
      ))}
    </>
  );
}

import { supabase } from "./supabase";

export async function checkDeadlines(user) {
  const now = new Date().toISOString();

  await supabase
    .from("tasks")
    .update({ status: "failed" })
    .eq("user_id", user.id)
    .eq("status", "pending")
    .lt("deadline", now);
}

import { useEffect, useState } from "react";
import { supabase } from "./supabase";
import CreateTask from "./CreateTask";
import TaskList from "./TaskList";
import { checkDeadlines } from "./checkDeadlines";

export default function Dashboard({ user }) {
  const [tasks, setTasks] = useState([]);

  const loadTasks = async () => {
    await checkDeadlines(user);

    const { data } = await supabase
      .from("tasks")
      .select("*")
      .order("created_at", { ascending: false });

    setTasks(data);
  };

  useEffect(() => {
    loadTasks();
  }, []);

  return (
    <div className="min-h-screen bg-bg text-white p-6 max-w-xl mx-auto">
      <h1 className="text-green text-2xl font-bold mb-6">FazHoje</h1>

      <CreateTask user={user} reload={loadTasks} />
      <TaskList tasks={tasks} reload={loadTasks} />
    </div>
  );
}

import { useEffect, useState } from "react";
import { supabase } from "./supabase";
import Auth from "./Auth";
import Dashboard from "./Dashboard";

export default function App() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    supabase.auth.getUser().then(({ data }) => {
      setUser(data?.user ?? null);
    });

    supabase.auth.onAuthStateChange((_event, session) => {
      setUser(session?.user ?? null);
    });
  }, []);

  return user ? <Dashboard user={user} /> : <Auth />;
}

npm install @stripe/stripe-js

VITE_STRIPE_PUBLIC_KEY=pk_test_xxxxxxxxx 

import { loadStripe } from "@stripe/stripe-js";

const stripePromise = loadStripe(import.meta.env.VITE_STRIPE_PUBLIC_KEY);

export default function Subscribe() {
  const subscribe = async () => {
    const stripe = await stripePromise;

    await stripe.redirectToCheckout({
      lineItems: [
        {
          price: "price_XXXXXXXX", // ID DO PLANO NO STRIPE
          quantity: 1,
        },
      ],
      mode: "subscription",
      successUrl: window.location.origin,
      cancelUrl: window.location.origin,
    });
  };

  return (
    <div className="bg-card p-6 rounded text-center mb-6">
      <h2 className="text-xl font-bold mb-3">Plano Ativo</h2>
      <p className="text-muted mb-4">
        R$29/mês para manter a disciplina
      </p>

      <button
        onClick={subscribe}
        className="bg-green text-black px-6 py-3 rounded font-bold"
      >
        Assinar agora
      </button>
    </div>
  );
}


alter table auth.users add column subscribed boolean default false;

import { supabase } from "./supabase";

export async function getRanking() {
  const { data } = await supabase.rpc("ranking_users");
  return data;
}

create or replace function ranking_users()
returns table (
  user_id uuid,
  score int
)
language sql
as $$
  select
    user_id,
    sum(
      case
        when status = 'done' then 1
        when status = 'failed' then -1
        else 0
      end
    ) as score
  from tasks
  group by user_id
  order by score desc;
$$;

import { useEffect, useState } from "react";
import { getRanking } from "./getRanking";

export default function Ranking() {
  const [ranking, setRanking] = useState([]);

  useEffect(() => {
    getRanking().then(setRanking);
  }, []);

  return (
    <div className="bg-card p-6 rounded mt-6">
      <h2 className="text-lg font-bold mb-4">🏆 Ranking</h2>

      {ranking.map((user, index) => (
        <div
          key={user.user_id}
          className="flex justify-between py-2 border-b border-border"
        >
          <span>#{index + 1}</span>
          <span className="text-green font-bold">
            {user.score}
          </span>
        </div>
      ))}
    </div>
  );
}

import Ranking from "./Ranking";
import Subscribe from "./Subscribe";

<Subscribe />
<Ranking />


import express from "express";
import Stripe from "stripe";
import bodyParser from "body-parser";
import { createClient } from "@supabase/supabase-js";

const app = express();
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY);

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_SERVICE_ROLE_KEY
);

app.post(
  "/webhook",
  bodyParser.raw({ type: "application/json" }),
  async (req, res) => {
    const sig = req.headers["stripe-signature"];

    let event;
    try {
      event = stripe.webhooks.constructEvent(
        req.body,
        sig,
        process.env.STRIPE_WEBHOOK_SECRET
      );
    } catch (err) {
      return res.status(400).send(`Webhook Error`);
    }

    if (event.type === "checkout.session.completed") {
      const session = event.data.object;
      const email = session.customer_email;

      await supabase
        .from("users")
        .update({ subscribed: true })
        .eq("email", email);
    }

    res.json({ received: true });
  }
);

app.listen(4242, () => console.log("Webhook rodando"));

STRIPE_SECRET_KEY=sk_test_xxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxx
SUPABASE_URL=https://xxxx.supabase.co
SUPABASE_SERVICE_ROLE_KEY=xxxxx

alter table auth.users
add column balance numeric default 0;


import { supabase } from "./supabase";

export default function AddBalance({ user }) {
  const add = async () => {
    await supabase
      .from("users")
      .update({ balance: user.balance + 50 })
      .eq("id", user.id);
  };

  return (
    <button
      onClick={add}
      className="w-full bg-green text-black py-2 rounded font-bold mb-4"
    >
      Adicionar R$50 de saldo
    </button>
  );
}


import { supabase } from "./supabase";

export async function checkDeadlines(user) {
  const now = new Date().toISOString();

  const { data: failedTasks } = await supabase
    .from("tasks")
    .select("*")
    .eq("user_id", user.id)
    .eq("status", "pending")
    .lt("deadline", now);

  for (let task of failedTasks) {
    await supabase.from("tasks")
      .update({ status: "failed" })
      .eq("id", task.id);

    await supabase.rpc("deduct_balance", {
      uid: user.id,
      amount: task.penalty,
    });
  }
}

create or replace function deduct_balance(uid uuid, amount numeric)
returns void
language plpgsql
as $$
begin
  update auth.users
  set balance = balance - amount
  where id = uid;
end;
$$;



export default function Landing() {
  return (
    <div className="min-h-screen bg-bg text-white flex items-center justify-center px-6">
      <div className="max-w-xl text-center">
        <h1 className="text-4xl font-bold mb-4">
          Faça hoje.<br />
          <span className="text-red">Ou pague por não fazer.</span>
        </h1>

        <p className="text-muted mb-6">
          O FazHoje cobra disciplina quando sua motivação falha.
        </p>

        <a
          href="/app"
          className="inline-block bg-green text-black px-8 py-4 rounded font-bold"
        >
          Começar agora
        </a>

        <p className="text-xs text-muted mt-4">
          Cancelamento em 1 clique.
        </p>
      </div>
    </div>
  );
}

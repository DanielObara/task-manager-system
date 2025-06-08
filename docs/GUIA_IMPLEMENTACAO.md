# Guia de Implementação do Desafio

Este guia fornece instruções passo a passo para a implementação do Sistema de Gerenciamento de Tarefas, projetado para ser concluído em até 2 horas.

## Pré-requisitos

- Node.js (versão 14+)
- npm ou yarn
- Editor de código (VS Code recomendado)
- Conhecimentos básicos de TypeScript, React e Node.js

## Estrutura do Projeto

```
task-manager/
├── client/                # Frontend React
│   ├── public/
│   ├── src/
│   │   ├── components/    # Componentes React
│   │   ├── interfaces/    # Definições de tipos TypeScript
│   │   ├── services/      # Serviços para comunicação com API
│   │   ├── App.tsx        # Componente principal
│   │   └── index.tsx      # Ponto de entrada
│   ├── package.json
│   └── tsconfig.json
│
├── server/                # Backend Node.js
│   ├── prisma/            # Definições do Prisma ORM
│   │   ├── schema.prisma  # Schema do banco de dados
│   │   └── migrations/    # Migrações do banco de dados
│   ├── src/
│   │   ├── controllers/   # Controladores da API
│   │   ├── routes/        # Definição de rotas
│   │   ├── services/      # Lógica de negócios
│   │   └── index.ts       # Ponto de entrada do servidor
│   ├── package.json
│   └── tsconfig.json
│
└── README.md              # Documentação do projeto
```

## Roteiro de Implementação

### Etapa 1: Configuração do Projeto (15 minutos)

1. **Criar a estrutura de pastas**
   ```bash
   mkdir -p task-manager/{client,server}
   ```

2. **Inicializar o frontend React com TypeScript**
   ```bash
   cd task-manager/client
   npx create-react-app . --template typescript
   ```

3. **Inicializar o backend Node.js com TypeScript**
   ```bash
   cd ../server
   npm init -y
   npm install express cors dotenv
   npm install -D typescript ts-node @types/node @types/express @types/cors
   npx tsc --init
   ```

4. **Configurar Prisma com SQLite**
   ```bash
   npm install prisma
   npm install @prisma/client
   npx prisma init --datasource-provider sqlite
   ```

### Etapa 2: Modelagem de Dados (15 minutos)

1. **Definir o modelo de tarefa no Prisma schema**
   Editar `server/prisma/schema.prisma`:
   ```prisma
   model Task {
     id          Int      @id @default(autoincrement())
     title       String
     description String?
     status      String   @default("pending")
     createdAt   DateTime @default(now())
     updatedAt   DateTime @updatedAt
   }
   ```

2. **Gerar a primeira migração**
   ```bash
   npx prisma migrate dev --name init
   ```

### Etapa 3: Implementação do Backend (30 minutos)

1. **Criar as rotas da API**
   Arquivo `server/src/routes/taskRoutes.ts`:
   ```typescript
   import express from 'express';
   import { TaskController } from '../controllers/taskController';

   const router = express.Router();
   const taskController = new TaskController();

   router.get('/', taskController.getAllTasks);
   router.get('/:id', taskController.getTaskById);
   router.post('/', taskController.createTask);
   router.put('/:id', taskController.updateTask);
   router.delete('/:id', taskController.deleteTask);

   export default router;
   ```

2. **Implementar o controlador de tarefas**
   Arquivo `server/src/controllers/taskController.ts`:
   ```typescript
   import { Request, Response } from 'express';
   import { PrismaClient } from '@prisma/client';

   const prisma = new PrismaClient();

   export class TaskController {
     async getAllTasks(req: Request, res: Response) {
       try {
         const tasks = await prisma.task.findMany();
         res.json(tasks);
       } catch (error) {
         res.status(500).json({ error: 'Erro ao buscar tarefas' });
       }
     }

     async getTaskById(req: Request, res: Response) {
       try {
         const { id } = req.params;
         const task = await prisma.task.findUnique({
           where: { id: Number(id) },
         });
         
         if (!task) {
           return res.status(404).json({ error: 'Tarefa não encontrada' });
         }
         
         res.json(task);
       } catch (error) {
         res.status(500).json({ error: 'Erro ao buscar tarefa' });
       }
     }

     async createTask(req: Request, res: Response) {
       try {
         const { title, description, status } = req.body;
         const newTask = await prisma.task.create({
           data: {
             title,
             description,
             status: status || 'pending',
           },
         });
         res.status(201).json(newTask);
       } catch (error) {
         res.status(500).json({ error: 'Erro ao criar tarefa' });
       }
     }

     async updateTask(req: Request, res: Response) {
       try {
         const { id } = req.params;
         const { title, description, status } = req.body;
         
         const updatedTask = await prisma.task.update({
           where: { id: Number(id) },
           data: {
             title,
             description,
             status,
           },
         });
         
         res.json(updatedTask);
       } catch (error) {
         res.status(500).json({ error: 'Erro ao atualizar tarefa' });
       }
     }

     async deleteTask(req: Request, res: Response) {
       try {
         const { id } = req.params;
         await prisma.task.delete({
           where: { id: Number(id) },
         });
         res.status(204).send();
       } catch (error) {
         res.status(500).json({ error: 'Erro ao excluir tarefa' });
       }
     }
   }
   ```

3. **Configurar o servidor Express**
   Arquivo `server/src/index.ts`:
   ```typescript
   import express from 'express';
   import cors from 'cors';
   import taskRoutes from './routes/taskRoutes';

   const app = express();
   const PORT = process.env.PORT || 5000;

   app.use(cors());
   app.use(express.json());

   app.use('/api/tasks', taskRoutes);

   app.listen(PORT, () => {
     console.log(`Servidor rodando na porta ${PORT}`);
   });
   ```

4. **Adicionar script de inicialização ao package.json**
   ```json
   "scripts": {
     "dev": "ts-node src/index.ts",
     "build": "tsc",
     "start": "node dist/index.js"
   }
   ```

### Etapa 4: Implementação do Frontend (45 minutos)

1. **Criar interfaces TypeScript**
   Arquivo `client/src/interfaces/Task.ts`:
   ```typescript
   export interface Task {
     id: number;
     title: string;
     description?: string;
     status: 'pending' | 'in-progress' | 'completed';
     createdAt: string;
     updatedAt: string;
   }

   export interface CreateTaskInput {
     title: string;
     description?: string;
     status: 'pending' | 'in-progress' | 'completed';
   }

   export interface UpdateTaskInput extends CreateTaskInput {
     id: number;
   }
   ```

2. **Implementar serviço de API**
   Arquivo `client/src/services/taskService.ts`:
   ```typescript
   import axios from 'axios';
   import { Task, CreateTaskInput, UpdateTaskInput } from '../interfaces/Task';

   const API_URL = 'http://localhost:5000/api/tasks';

   export const TaskService = {
     getAllTasks: async (): Promise<Task[]> => {
       const response = await axios.get(API_URL);
       return response.data;
     },

     getTaskById: async (id: number): Promise<Task> => {
       const response = await axios.get(`${API_URL}/${id}`);
       return response.data;
     },

     createTask: async (task: CreateTaskInput): Promise<Task> => {
       const response = await axios.post(API_URL, task);
       return response.data;
     },

     updateTask: async (task: UpdateTaskInput): Promise<Task> => {
       const response = await axios.put(`${API_URL}/${task.id}`, task);
       return response.data;
     },

     deleteTask: async (id: number): Promise<void> => {
       await axios.delete(`${API_URL}/${id}`);
     }
   };
   ```

3. **Criar componentes React**
   
   **TaskList.tsx**
   ```typescript
   import React, { useState, useEffect } from 'react';
   import { Task } from '../interfaces/Task';
   import { TaskService } from '../services/taskService';
   import TaskItem from './TaskItem';

   const TaskList: React.FC = () => {
     const [tasks, setTasks] = useState<Task[]>([]);
     const [loading, setLoading] = useState(true);
     const [error, setError] = useState('');

     useEffect(() => {
       loadTasks();
     }, []);

     const loadTasks = async () => {
       try {
         setLoading(true);
         const data = await TaskService.getAllTasks();
         setTasks(data);
         setError('');
       } catch (err) {
         setError('Erro ao carregar tarefas');
         console.error(err);
       } finally {
         setLoading(false);
       }
     };

     const handleDelete = async (id: number) => {
       try {
         await TaskService.deleteTask(id);
         setTasks(tasks.filter(task => task.id !== id));
       } catch (err) {
         setError('Erro ao excluir tarefa');
         console.error(err);
       }
     };

     if (loading) return <div>Carregando tarefas...</div>;
     if (error) return <div>Erro: {error}</div>;

     return (
       <div className="task-list">
         <h2>Minhas Tarefas</h2>
         {tasks.length === 0 ? (
           <p>Nenhuma tarefa encontrada.</p>
         ) : (
           tasks.map(task => (
             <TaskItem 
               key={task.id} 
               task={task} 
               onDelete={handleDelete} 
               onStatusUpdate={loadTasks}
             />
           ))
         )}
       </div>
     );
   };

   export default TaskList;
   ```

   **TaskItem.tsx**
   ```typescript
   import React from 'react';
   import { Task } from '../interfaces/Task';
   import { TaskService } from '../services/taskService';

   interface TaskItemProps {
     task: Task;
     onDelete: (id: number) => void;
     onStatusUpdate: () => void;
   }

   const TaskItem: React.FC<TaskItemProps> = ({ task, onDelete, onStatusUpdate }) => {
     const handleStatusChange = async (e: React.ChangeEvent<HTMLSelectElement>) => {
       try {
         await TaskService.updateTask({
           ...task,
           status: e.target.value as 'pending' | 'in-progress' | 'completed'
         });
         onStatusUpdate();
       } catch (err) {
         console.error('Erro ao atualizar status:', err);
       }
     };

     return (
       <div className={`task-item status-${task.status}`}>
         <h3>{task.title}</h3>
         {task.description && <p>{task.description}</p>}
         <div className="task-actions">
           <select 
             value={task.status} 
             onChange={handleStatusChange}
           >
             <option value="pending">Pendente</option>
             <option value="in-progress">Em Andamento</option>
             <option value="completed">Concluída</option>
           </select>
           <button onClick={() => onDelete(task.id)}>Excluir</button>
         </div>
       </div>
     );
   };

   export default TaskItem;
   ```

   **TaskForm.tsx**
   ```typescript
   import React, { useState } from 'react';
   import { CreateTaskInput } from '../interfaces/Task';
   import { TaskService } from '../services/taskService';

   interface TaskFormProps {
     onTaskCreated: () => void;
   }

   const TaskForm: React.FC<TaskFormProps> = ({ onTaskCreated }) => {
     const [title, setTitle] = useState('');
     const [description, setDescription] = useState('');
     const [status, setStatus] = useState<'pending' | 'in-progress' | 'completed'>('pending');
     const [isSubmitting, setIsSubmitting] = useState(false);
     const [error, setError] = useState('');

     const handleSubmit = async (e: React.FormEvent) => {
       e.preventDefault();
       
       if (!title.trim()) {
         setError('O título é obrigatório');
         return;
       }

       try {
         setIsSubmitting(true);
         setError('');
         
         const newTask: CreateTaskInput = {
           title,
           description: description || undefined,
           status
         };
         
         await TaskService.createTask(newTask);
         
         // Limpar formulário
         setTitle('');
         setDescription('');
         setStatus('pending');
         
         // Notificar componente pai
         onTaskCreated();
       } catch (err) {
         setError('Erro ao criar tarefa');
         console.error(err);
       } finally {
         setIsSubmitting(false);
       }
     };

     return (
       <div className="task-form-container">
         <h2>Nova Tarefa</h2>
         {error && <div className="error-message">{error}</div>}
         <form onSubmit={handleSubmit}>
           <div className="form-group">
             <label htmlFor="title">Título:</label>
             <input
               type="text"
               id="title"
               value={title}
               onChange={(e) => setTitle(e.target.value)}
               disabled={isSubmitting}
               required
             />
           </div>
           
           <div className="form-group">
             <label htmlFor="description">Descrição:</label>
             <textarea
               id="description"
               value={description}
               onChange={(e) => setDescription(e.target.value)}
               disabled={isSubmitting}
             />
           </div>
           
           <div className="form-group">
             <label htmlFor="status">Status:</label>
             <select
               id="status"
               value={status}
               onChange={(e) => setStatus(e.target.value as any)}
               disabled={isSubmitting}
             >
               <option value="pending">Pendente</option>
               <option value="in-progress">Em Andamento</option>
               <option value="completed">Concluída</option>
             </select>
           </div>
           
           <button type="submit" disabled={isSubmitting}>
             {isSubmitting ? 'Salvando...' : 'Criar Tarefa'}
           </button>
         </form>
       </div>
     );
   };

   export default TaskForm;
   ```

4. **Montar aplicação principal**
   `client/src/App.tsx`:
   ```typescript
   import React, { useState } from 'react';
   import './App.css';
   import TaskList from './components/TaskList';
   import TaskForm from './components/TaskForm';

   function App() {
     const [refreshTasks, setRefreshTasks] = useState(0);

     const handleTaskCreated = () => {
       // Incrementar para forçar atualização da lista
       setRefreshTasks(prev => prev + 1);
     };

     return (
       <div className="App">
         <header className="App-header">
           <h1>Gerenciador de Tarefas</h1>
         </header>
         <main>
           <div className="container">
             <TaskForm onTaskCreated={handleTaskCreated} />
             <TaskList key={refreshTasks} />
           </div>
         </main>
         <footer>
           <p>Desafio de Desenvolvimento - 2025</p>
         </footer>
       </div>
     );
   }

   export default App;
   ```

5. **Adicionar estilos CSS básicos**
   Modificar `client/src/App.css` com estilos básicos para a aplicação.

### Etapa 5: Teste e Depuração (15 minutos)

1. **Iniciar o backend**
   ```bash
   cd server
   npm run dev
   ```

2. **Iniciar o frontend**
   ```bash
   cd ../client
   npm start
   ```

3. **Testar as operações CRUD**
   - Criar algumas tarefas
   - Atualizar o status de tarefas
   - Excluir tarefas
   - Verificar se as mudanças persistem entre recarregamentos

### Etapa 6: Implementação de Testes (30 minutos)

1. **Configurar Jest para o backend**
   ```bash
   cd server
   npm install --save-dev jest ts-jest @types/jest supertest @types/supertest
   ```
   
   Criar arquivo `jest.config.js`:
   ```javascript
   module.exports = {
     preset: 'ts-jest',
     testEnvironment: 'node',
     testMatch: ['**/__tests__/**/*.test.ts'],
     transform: {
       '^.+\\.ts$': 'ts-jest',
     },
     collectCoverage: true,
     coverageDirectory: 'coverage',
     collectCoverageFrom: [
       'src/**/*.ts',
       '!src/**/*.d.ts',
     ],
   };
   ```

2. **Criar testes para o controller de tarefas**
   Criar a estrutura de pastas para testes:
   ```bash
   mkdir -p __tests__/{controllers,mocks}
   ```
   
   Criar um mock para o PrismaClient:
   ```typescript
   // __tests__/mocks/prismaMock.ts
   import { PrismaClient } from '@prisma/client';
   
   const prismaMock = {
     task: {
       findMany: jest.fn(),
       findUnique: jest.fn(),
       create: jest.fn(),
       update: jest.fn(),
       delete: jest.fn(),
     }
   } as unknown as PrismaClient;
   
   export default prismaMock;
   ```
   
   Implementar testes para o controller:
   ```typescript
   // __tests__/controllers/taskController.test.ts
   import { Request, Response } from 'express';
   import { TaskController } from '../../src/controllers/taskController';
   import prismaMock from '../mocks/prismaMock';
   
   // Mock do PrismaClient
   jest.mock('@prisma/client', () => {
     return {
       PrismaClient: jest.fn(() => prismaMock)
     };
   });
   
   describe('TaskController', () => {
     let taskController: TaskController;
     let mockRequest: Partial<Request>;
     let mockResponse: Partial<Response>;
     
     beforeEach(() => {
       // Configure mocks e controller
       // ...
     });
     
     describe('getAllTasks', () => {
       it('deve retornar todas as tarefas com status 200', async () => {
         // Implemente o teste
       });
     });
     
     // Mais testes para outros métodos
   });
   ```

3. **Configurar testes para o frontend**
   O Create React App já vem com Jest configurado, então só precisamos criar os testes:
   ```bash
   cd ../client
   mkdir -p src/__tests__/{components,services}
   ```
   
   Implementar testes para o serviço de tarefas:
   ```typescript
   // src/__tests__/services/taskService.test.ts
   import axios from 'axios';
   import { TaskService } from '../../services/taskService';
   
   jest.mock('axios');
   const mockedAxios = axios as jest.Mocked<typeof axios>;
   
   describe('TaskService', () => {
     beforeEach(() => {
       jest.clearAllMocks();
     });
     
     describe('getAllTasks', () => {
       it('deve retornar todas as tarefas', async () => {
         // Implemente o teste
       });
     });
     
     // Mais testes para outros métodos
   });
   ```
   
   Implementar testes para componentes React:
   ```typescript
   // src/__tests__/components/TaskList.test.tsx
   import React from 'react';
   import { render, screen } from '@testing-library/react';
   import TaskList from '../../components/TaskList';
   import { TaskService } from '../../services/taskService';
   
   jest.mock('../../services/taskService');
   
   describe('TaskList', () => {
     it('deve exibir a lista de tarefas', async () => {
       // Implemente o teste
     });
     
     // Mais testes para diferentes cenários
   });
   ```

4. **Executar os testes**
   ```bash
   # Backend
   cd server
   npm test
   
   # Frontend
   cd ../client
   npm test
   ```

### Próximos passos (opcional - se houver tempo adicional)

1. **Adicionar filtros para tarefas por status**
2. **Implementar pesquisa de tarefas**
3. **Adicionar animações para melhor experiência do usuário**
4. **Implementar validações adicionais**

## Dicas para os Alunos

- Comece sempre pelo backend e garanta que a API esteja funcionando antes de implementar o frontend
- Use ferramentas como Postman ou o endpoint `/api/tasks` diretamente no navegador para testar a API
- Lembre-se de instalar as dependências necessárias (como `axios` para o frontend)
- O Prisma facilita muito as operações de banco de dados, aproveite sua documentação
- Cometa frequentemente para não perder progresso

## Resolução de Problemas Comuns

- **CORS**: Certifique-se de que o middleware cors está configurado corretamente
- **Erros do Prisma**: Verifique se as migrações foram aplicadas corretamente
- **Problemas de Tipagem**: Use as interfaces TypeScript definidas para garantir consistência
- **Comunicação API**: Verifique se a URL da API está correta no serviço do frontend

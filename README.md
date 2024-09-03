# Todo List App

This is a simple Todo List application built with **React**, **Vite**, and **Tailwind CSS**. The project serves as a practical exercise in state management, component architecture, and integrating a utility-first CSS framework. It is particularly suited for beginners looking to solidify their understanding of React development.

## Features

- **Add Todos**: Users can add new tasks to their list.
- **Remove Todos**: Users can remove tasks once they're completed or no longer needed.
- **Todo Summary**: A summary of the total tasks and completed tasks.
- **Responsive Design**: The app is styled with Tailwind CSS to ensure a responsive and clean UI.

## Project Structure

The application is organized into several key components and hooks:

### Components

1. **`AddTodoForm.tsx`**: 
   - **Purpose**: Manages the form for adding new todos.
   - **Key Points**:
     - It uses React's `useState` to manage the input field's state.
     - The form submission is handled through an `onSubmit` event, which triggers a function passed down from the parent component to add a new todo.

   ```tsx
   const [todo, setTodo] = useState("");

   const handleSubmit = (e: React.FormEvent) => {
       e.preventDefault();
       if (todo.trim()) {
           onAddTodo(todo);
           setTodo("");
       }
   };
   ```

2. **`TodoItem.tsx`**:
   - **Purpose**: Represents a single todo item.
   - **Key Points**:
     - This component receives the todo data and a function to remove the todo as props.
     - It uses event handling to trigger the removal function when the delete button is clicked.

   ```tsx
   return (
       <li>
           {todo.text}
           <button onClick={() => onRemoveTodo(todo.id)}>Delete</button>
       </li>
   );
   ```

3. **`TodoList.tsx`**:
   - **Purpose**: Renders a list of todo items.
   - **Key Points**:
     - Maps over the list of todos and renders each `TodoItem`.
     - This component is purely presentational and doesn't handle any state management itself.

   ```tsx
   return (
       <ul>
           {todos.map(todo => (
               <TodoItem key={todo.id} todo={todo} onRemoveTodo={onRemoveTodo} />
           ))}
       </ul>
   );
   ```

4. **`TodoSummary.tsx`**:
   - **Purpose**: Displays a summary of todos, such as the total count and completed count.
   - **Key Points**:
     - Leverages simple derived state by calculating counts directly within the component based on the `todos` prop.

   ```tsx
   const totalTodos = todos.length;
   const completedTodos = todos.filter(todo => todo.completed).length;

   return (
       <div>
           <p>Total Todos: {totalTodos}</p>
           <p>Completed Todos: {completedTodos}</p>
       </div>
   );
   ```

### Hooks

1. **`useTodos.ts`**:
   - **Purpose**: Manages the state of the todo list.
   - **Key Points**:
     - This custom hook encapsulates all the logic for adding, removing, and managing the state of the todos. 
     - It utilizes `useState` and `useEffect` for state management and side effects.

   ```tsx
   const [todos, setTodos] = useState<Todo[]>([]);

   const addTodo = (text: string) => {
       setTodos(prevTodos => [...prevTodos, { id: Date.now(), text, completed: false }]);
   };

   const removeTodo = (id: number) => {
       setTodos(prevTodos => prevTodos.filter(todo => todo.id !== id));
   };

   return { todos, addTodo, removeTodo };
   ```

2. **`todos.ts`**:
   - **Purpose**: Contains type definitions and initial state for the todo list.
   - **Key Points**:
     - Defines the structure of a Todo item using TypeScript's type system.
     - Sets up an initial state for todos, which can be used to populate the list on first render.

   ```ts
   export type Todo = {
       id: number;
       text: string;
       completed: boolean;
   };
   ```

## Best Practices

### 1. **Component Decomposition**
   - **Explanation**: Break down your UI into the smallest logical components. Each component should have a single responsibility, making it easier to manage, test, and reuse.
   - **Example**: The `TodoItem.tsx` component focuses only on rendering a single todo item, while the `TodoList.tsx` component focuses on rendering the list. This separation of concerns ensures that each component has a clear and focused role.

### 2. **State Management with Custom Hooks**
   - **Explanation**: Using custom hooks for state management, like `useTodos.ts`, abstracts away the logic from your UI components. This keeps components clean and focused on presentation while making your logic reusable.
   - **Example**: `useTodos` encapsulates the logic for adding and removing todos, which means if another component needed to manage todos in the future, you could simply reuse this hook without duplicating code.

### 3. **Prop-Drilling Avoidance**
   - **Explanation**: While this project is small, in larger applications, deeply nested components passing props can become unmanageable. Consider using context or state management libraries like Redux or Zustand if you find yourself passing props through multiple levels.
   - **Example**: In this app, props are passed directly from parent components to children (e.g., `TodoList` passes a remove function to `TodoItem`). This is manageable here but can become complex as your app scales.

### 4. **Styling with Tailwind CSS**
   - **Explanation**: Tailwind CSS provides utility-first classes that allow you to style your components directly within your JSX, keeping your styles consistent and minimizing the need for custom CSS.
   - **Best Practice**: Stick to Tailwind's utility classes as much as possible. Only create custom styles if necessary, which reduces the complexity of maintaining separate CSS files.
   - **Example**: The `index.css` file is minimal and primarily imports Tailwind's base styles, components, and utilities, avoiding unnecessary custom styles.

   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

### 5. **Vite for Development**
   - **Explanation**: Vite is a powerful tool for React development, offering faster build times and hot module replacement. It’s particularly advantageous when working on projects that require frequent updates and testing.
   - **Best Practice**: Use Vite's fast refresh capabilities to iterate quickly during development. Regularly build for production to ensure that the optimizations work as expected.

## Summary

This project is a straightforward example of how to structure a React application using best practices. By focusing on component decomposition, state management, and leveraging Tailwind CSS for styling, this app serves as a solid foundation for learning and building more complex applications in the future.
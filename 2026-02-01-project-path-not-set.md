often times, when a project is initialized, claude code does not have context of the project path (this is what I suspect). So instead of reading the project directory to understand what to do, it asks clarifying questions. When I send a second message, it figures out the context, this should not happen. The project context including files should be available from the start

```html
<div class="messages-list">
  <div class="message-item user" style="opacity: 1; transform: none">
    <div class="user-message"><div class="user-message-text">create a todo list</div></div>
  </div>
  <div class="message-item assistant" style="opacity: 1; transform: none">
    <div class="assistant-message">
      <div class="assistant-text">
        <div class="space-y-4 whitespace-normal *:first:mt-0 *:last:mb-0">
          <p>Sure! Could you tell me more about what you'd like? For example:</p>
          <ul class="list-inside list-disc whitespace-normal [li_&amp;]:pl-6" data-streamdown="unordered-list">
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">
              A <span class="font-semibold" data-streamdown="strong">todo list app/feature</span> in a project? (If so,
              what tech stack or project are you working with?)
            </li>
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">
              A <span class="font-semibold" data-streamdown="strong">task list for me to track</span> while working on
              something for you?
            </li>
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">Something else?</li>
          </ul>
          <p>Let me know and I'll get started!</p>
        </div>
      </div>
    </div>
  </div>
  <div class="message-item user" style="opacity: 1; transform: none">
    <div class="user-message"><div class="user-message-text">Read the current directory</div></div>
  </div>
  <div class="message-item assistant" style="opacity: 1; transform: none">
    <div class="assistant-message">
      <div class="tool-accordion-single">
        <div class="tool-pill-compact" style="opacity: 1; transform: none">
          <span class="tool-pill-compact-icon"
            ><svg
              xmlns="http://www.w3.org/2000/svg"
              width="14"
              height="14"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-terminal"
              aria-hidden="true"
            >
              <path d="M12 19h8"></path>
              <path d="m4 17 6-6-6-6"></path></svg></span
          ><span class="tool-pill-compact-action">Executing command</span
          ><span class="tool-pill-compact-label" title="ls">ls</span>
        </div>
      </div>
      <div class="assistant-text">
        <div class="space-y-4 whitespace-normal *:first:mt-0 *:last:mb-0">
          <p>
            This is an <span class="font-semibold" data-streamdown="strong">Expo + React Native</span> project with
            <span class="font-semibold" data-streamdown="strong">NativeWind/Tailwind CSS</span>. I'd be happy to create
            a todo list feature for it!
          </p>
          <p>Would you like me to go ahead and plan this out? I'm thinking:</p>
          <ul class="list-inside list-disc whitespace-normal [li_&amp;]:pl-6" data-streamdown="unordered-list">
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">
              A <span class="font-semibold" data-streamdown="strong">Todo List screen</span> with the ability to add,
              complete, and delete tasks
            </li>
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">
              Styled with <span class="font-semibold" data-streamdown="strong">Tailwind CSS (NativeWind)</span>
            </li>
            <li class="py-1 [&amp;>p]:inline" data-streamdown="list-item">
              Using <span class="font-semibold" data-streamdown="strong">local state</span> (or would you prefer
              persistent storage like AsyncStorage?)
            </li>
          </ul>
          <p>
            Shall I proceed, or do you have specific requirements (e.g., design, navigation placement, backend
            integration)?
          </p>
        </div>
      </div>
    </div>
  </div>
  <div></div>
</div>

```
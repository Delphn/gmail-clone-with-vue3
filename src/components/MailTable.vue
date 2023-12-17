<template>
    <h1>VMail Inbox</h1>
    <table class="mail-table">
      <tbody>
        <tr v-for="email in unArchivedEmails"
            :key="email.id"
            :class="['clickable', email.read ? 'read' :  '']"
            @click="email.read = true">
            <td>
              <input type="checkbox"
                     :checked="email.selected"
                     @click="email.selected = !email.selected">
            </td>
            <td>{{ email.from }}</td>
            <td>
              <p><strong>{{ email.subject }}</strong> - {{ email.body }}</p>
            </td>
            <td class="date">{{ format(new Date(email.sentAt), 'MMM do yyy') }}</td>
            <td><button @click="email.archived = true">Archive</button></td>
        </tr>
      </tbody>
    </table>
</template>

<script>
import axios from 'axios';
import { format } from 'date-fns';

export default {
    async setup(){
        const { data: emails } = await axios.get('http://localhost:3000/emails');
        return {
            format,
            emails
        }
  },
  computed: {
    sortedEmails() {
      return this.emails.sort((a, b) => {
        return a.sentAt < b.sentAt ? 1 : -1;
      });
    },
    unArchivedEmails() {
      return this.sortedEmails.filter(email => !email.archived);
    },
  }
}
</script>
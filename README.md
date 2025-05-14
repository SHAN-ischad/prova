# Exemplo de Navegação Completa com Formulário e AsyncStorage

Este projeto demonstra como implementar uma navegação completa entre telas utilizando o [expo-router](https://docs.expo.dev/router/introduction/), um formulário simples e o armazenamento de dados local usando [AsyncStorage](https://react-native-async-storage.github.io/async-storage/).

## Funcionalidades

- Navegação entre telas (Home, Formulário, Detalhes)
- Formulário para inserir nome e e-mail
- Salvamento dos dados do formulário no AsyncStorage
- Listagem dos dados salvos

## Instalação

1. Instale as dependências:
   ```sh
   npm install
   ```

2. Instale o AsyncStorage:
   ```sh
   npx expo install @react-native-async-storage/async-storage
   ```

3. Inicie o projeto:
   ```sh
   npx expo start
   ```

## Estrutura de Arquivos

```
src/
  app/
    _layout.tsx
    index.tsx         # Tela inicial (Home)
    form.tsx          # Tela do formulário
    details.tsx       # Tela de detalhes
```

## Exemplo de Código

### Navegação (_layout.tsx)

```tsx
// filepath: src/app/_layout.tsx
import { Stack } from "expo-router";

export default function RootLayout() {
  return <Stack />;
}
```

### Tela Inicial (index.tsx)

```tsx
// filepath: src/app/index.tsx
import { Link } from "expo-router";
import { View, Text, Button } from "react-native";

export default function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      <Text>Bem-vindo!</Text>
      <Link href="/form" asChild>
        <Button title="Ir para Formulário" />
      </Link>
      <Link href="/details" asChild>
        <Button title="Ver Detalhes" />
      </Link>
    </View>
  );
}
```

### Formulário (form.tsx)

```tsx
// filepath: src/app/form.tsx
import { useState } from "react";
import { View, TextInput, Button, Alert } from "react-native";
import AsyncStorage from "@react-native-async-storage/async-storage";
import { useRouter } from "expo-router";

export default function FormScreen() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const router = useRouter();

  const saveData = async () => {
    if (!name || !email) {
      Alert.alert("Preencha todos os campos!");
      return;
    }
    await AsyncStorage.setItem("user", JSON.stringify({ name, email }));
    Alert.alert("Dados salvos com sucesso!");
    router.push("/details");
  };

  return (
    <View style={{ flex: 1, justifyContent: "center", padding: 20 }}>
      <TextInput
        placeholder="Nome"
        value={name}
        onChangeText={setName}
        style={{ borderWidth: 1, marginBottom: 10, padding: 8 }}
      />
      <TextInput
        placeholder="E-mail"
        value={email}
        onChangeText={setEmail}
        keyboardType="email-address"
        style={{ borderWidth: 1, marginBottom: 10, padding: 8 }}
      />
      <Button title="Salvar" onPress={saveData} />
    </View>
  );
}
```

### Detalhes (details.tsx)

```tsx
// filepath: src/app/details.tsx
import { useEffect, useState } from "react";
import { View, Text } from "react-native";
import AsyncStorage from "@react-native-async-storage/async-storage";

export default function DetailsScreen() {
  const [user, setUser] = useState<{ name: string; email: string } | null>(null);

  useEffect(() => {
    AsyncStorage.getItem("user").then((data) => {
      if (data) setUser(JSON.parse(data));
    });
  }, []);

  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      {user ? (
        <>
          <Text>Nome: {user.name}</Text>
          <Text>E-mail: {user.email}</Text>
        </>
      ) : (
        <Text>Nenhum dado encontrado.</Text>
      )}
    </View>
  );
}
```

## Observações

- O AsyncStorage é usado para persistir os dados localmente no dispositivo.
- O [expo-router](https://docs.expo.dev/router/introduction/) facilita a navegação baseada em arquivos.
- Para mais informações, consulte a [documentação do AsyncStorage](https://react-native-async-storage.github.io/async-storage/docs/usage/).

---

# Aula 03 - In Orbit

---

## Services

Serviços são onde as regras de negócio da classe irão rodar.
Exemplo:

`SettingsService.ts`

```ts
import { getCustomRepository } from 'typeorm';
import { SettingsRepository } from '../repositories/SettingsRepository';

interface ISettingsCreate {
  chat: boolean;
  username: string;
}

class SettingsService {
  async create({ chat, username }: ISettingsCreate) {
    const settingsRepository = getCustomRepository(SettingsRepository);

    // SELECT * FROM SETTINGS WHERE USERNAME = 'username' LIMIT 1
    const userAlreadyExists = await settingsRepository.findOne({
      username,
    });

    if (userAlreadyExists) {
      throw new Error('User already exists');
    }

    const settings = settingsRepository.create({
      chat,
      username,
    });

    await settingsRepository.save(settings);

    return settings;
  }
}

export { SettingsService };
```

# Mobile game

Owns native Android/iOS gameplay: combat, AI, movement, animation, arena presentation, cameras, audio, local replay, training and touch/controller input.

Engine and launch OS order are pending evaluation. Place the selected engine's normal project layout here. Separate combat rules from presentation and input. Add feature folders only when implementing them, and document the build and device-test commands here.

Use `shared/` contracts for online services. Fully disconnected AI play remains local; spendable online wallet state is server-owned. Windows builds may support development, without implying a public PC release.

Large game assets require a deliberate Git LFS or external asset-storage policy before import. Binary attributes alone do not enable LFS.

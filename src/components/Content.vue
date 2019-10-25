<template>
	<main role="main">
			<div class="jumbotron">
				<h2>Novo orçamento <span class="float-right">({{ dataAtual }})</span></h2>
				<p class="lead">Prencha os dados abaixo para obter os valores.</p>

				<div class="row">
					<div class="col-md-4 order-md-2 mb-4">
						<h4 class="d-flex justify-content-between align-items-center mb-3">
							<span class="text-muted">Orçamento</span>
							<span class="badge badge-secondary badge-pill">{{ items.length }}</span>
						</h4>
						<ul class="list-group mb-3">
							<li class="list-group-item d-flex justify-content-between lh-condensed" v-for="item in items" 
								v-bind:key="item.descricao">
								<div>
									<h6 class="my-0">{{ item.tipoPeca }} <small>{{item.altura}} X {{item.comprimento}} <b>({{ item.metragem }} m²)</b></small></h6>
									<small class="text-muted">{{ item.descricao }}</small>
								</div>
								<span class="text-muted">R$ {{ item.preco }}</span>
							</li>
						</ul>
						<h3>
							Total {{ total }}
						</h3>
					</div>
					<div class="col-md-8 order-md-1">
						<h4 class="mb-3">Cadastro pessoal...</h4>
												
							<div class="mb-3">
								<label for="firstName">Cliente:</label>
								<input type="text" class="form-control" id="firstName" placeholder="Nome e sobrenome" value="" required="">
								<div class="invalid-feedback">
									O nome é obrigatório
								</div>
							</div>
					
							<div class="row">
									<div class="col-md-4">
										<label for="tipoPeca">Tipo</label>
										<select id="tipoPeca" class="form-control" v-model="item.tipoPeca">
											<option value="">Selecione o tipo da peça</option>
											<optgroup label="Peças">
												<option>Cuba</option>
												<option>Pia</option>
												<option>Outros...</option>
											</optgroup>
											<optgroup label="Individual">
												<option>Pingadeira</option>
												<option>Espelho</option>
												<option>Rodapé</option>
												<option>Soleira</option>
											</optgroup>
										</select>
									</div>

									<div class="col-md-4">
											<label for="altura">Altura (metros)</label>
											<input type="number" id="altura" placeholder="0" class="form-control" 
											v-model="item.altura" step=".01" />
									</div>

									<div class="col-md-4">
											<label for="comprimento">Comprimento (metros)</label>
											<input type="number" id="comprimento" placeholder="0" class="form-control" 
											v-model="item.comprimento" step=".01" />
									</div>		
							</div>	

							<div class="row">
								<div class="col-md-6 mb-3">
									<label for="firstName">Descrição da peça:</label>
									<input type="text" class="form-control" id="firstName" 
										placeholder="Informação da peça" v-model="item.descricao">
									<div class="invalid-feedback">
										A informação da peça é obrigatória
									</div>
								</div>

								<div class="col-md-3">
									<label for="acabamento">Acabamento</label>
									<select class="form-control" id="acabamento" v-model="item.acabamento">
										<option>Selecione o acabamento</option>
										<option>Linear</option>
										<option>Meia esquadria</option>
										<option>Duplo lixado</option>
										<option>Outras...</option>
									</select>
								</div>

								<div class="col-md-3">
									<label for="cor">Cor</label>
									<select class="form-control" id="cor" v-model="item.cor">
										<option value="0">Selecione a cor</option>
										<option value="1">Ocre (R$ 152,15)</option>
										<option value="2">Preto absoluto (R$ 327,00)</option>
										<option value="3">Preto (R$ 245,25)</option>
										<option value="4">Verde Ubatuba (R$ 190,15)</option>
										<option value="5">Cinza (R$ 300,89)</option>
										<option value="6">Outras...</option>
									</select>
								</div>

								<div class="col-md-12" id="area">
									<div>
										Metragem: <span class="badge badge-inverse">0 m2</span>
									</div>
								</div>
								<div class="text-center">
									<button class="btn btn-success" 
											title="Adicionar novo item na lista" 
											v-on:click="addItem(item)">
											Adicionar
									</button>
								</div>
							</div>				
							
							<hr class="mb-4">
							<div class="d-block my-3">
								<div class="custom-control custom-radio">
									<input id="retirar-endereco" name="opcao-entrega" type="radio" 
									class="custom-control-input" checked="" required="">
									<label class="custom-control-label" for="retirar-endereco">Retirar na loja</label>
								</div>
								<div class="custom-control custom-radio">
									<input id="entrega-loja" name="opcao-entrega" type="radio" 
									class="custom-control-input" 
									required="">
									<label class="custom-control-label" for="entrega-loja">Entregar no endereço</label>
									<div>
										<small class="text-muted">Será acrescido um valor de R$ 35,00 de frete!</small>
									</div>
								</div>
							</div>

							<div class="custom-control custom-checkbox">
								<input type="checkbox" class="custom-control-input" id="save-info">
								<label class="custom-control-label" for="save-info">Salvar este orçamento para depois</label>

							</div>
							<hr class="mb-4">

							<h4 class="mb-3">Forma de pagamento</h4>

							<div class="d-block my-3">
								<div class="custom-control custom-radio">
									<input id="credit" name="paymentMethod" type="radio" class="custom-control-input" checked="" required="">
									<label class="custom-control-label" for="credit">Cartão de crédito</label>
								</div>
								<div class="custom-control custom-radio">
									<input id="debit" name="paymentMethod" type="radio" class="custom-control-input" required="">
									<label class="custom-control-label" for="debit">Cartão de débito</label>
								</div>
								<div class="custom-control custom-radio">
									<input id="paypal" name="paymentMethod" type="radio" class="custom-control-input" required="">
									<label class="custom-control-label" for="paypal">Cheques</label>
								</div>
							</div>
							
							<hr class="mb-4">
							<button class="btn btn-primary btn-lg btn-block" type="submit">Finalizar o orçamento</button>
						
					</div>
				</div>
				
			</div>
	</main>
</template>

<script>
export default {
  name: 'Content',
  props: {},
	data: function(){
		return {
			item : {},
			items : [],
			dataAtual : new Date().toLocaleString(),
			total : 0
		}
	},
	methods: {
		addItem(item){
			let metragem = item.altura * item.comprimento;
			item.metragem = metragem.toFixed(2); // / 1000;
			let precoPorMetroQuadrado = 0;
			if(item.cor === 1) 
				precoPorMetroQuadrado = 152.15;
			else if(item.cor === 2)
				precoPorMetroQuadrado = 327;
			else if(item.cor === 3)
				precoPorMetroQuadrado = 245.25;
			else if(item.cor === 4)
				precoPorMetroQuadrado = 190.15;
			else precoPorMetroQuadrado = 300.89;
							
			let valor = item.metragem * precoPorMetroQuadrado;
			item.preco = valor.toFixed(2);
		
			this.items.push(item);
			this.item = {}
			this.total = parseFloat(this.total) + parseFloat(item.preco);
		}
	}
}
</script>

<style>
</style>
<template>
	<main role="main">
			<div class="jumbotron">
				<div class="container">
					<h5>Novo orçamento <span class="float-right">({{ dataAtual }})</span></h5>
					<p class="lead">Prencha os dados abaixo para obter os valores.</p>
				</div>
			
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
								<span class="text-muted">{{ item.quantidade }} X</span>
								<span class="text-muted">R$ {{ item.preco }}</span>
							</li>
						</ul>
						<h3>
							Total {{ total }}
						</h3>
					</div>
					<div class="col-md-8 order-md-1">
												
							<div class="mb-3">
								<label for="firstName">Cliente:</label>
								<input type="text" class="form-control" id="firstName" 
								ref="nome" v-model="cliente"
								placeholder="Nome e sobrenome" value="" required="">
								<div class="invalid-feedback">
									O nome é obrigatório
								</div>
							</div>

						<div class="row">
							<div class="col-md-6">
									<label for="cor">Cor da pedra</label>
									<select class="form-control" id="cor" v-model="pedra" ref="pedra">
										<option value="0">Selecione a cor...</option>
										<option value="1">Ocre (R$ 152,15)</option>
										<option value="2">Preto absoluto (R$ 327,00)</option>
										<option value="3">Preto (R$ 245,25)</option>
										<option value="4">Verde Ubatuba (R$ 190,15)</option>
										<option value="5">Cinza (R$ 300,89)</option>
										<option value="6">Outras...</option>
									</select>
								</div>
									
									<div class="col-md-6">
										<label for="pecaProduto">Produto/Peça</label>
										<select id="pecaProduto" class="form-control" v-model="item.tipoPeca" 
										ref="produto"
										@change="alterarTipoProduto(item.tipoPeca)">
											<option value="">Selecione o tipo da peça</option>
											<optgroup label="Produtos">
												<option value="1">Cuba</option>
												<option value="2">Pia</option>
												<option value="3">Outros...</option>
											</optgroup>
											<optgroup label="Peças">
												<option value="4">Pingadeira 1.02 x 16</option>
												<option value="5">Pingadeira 1.22 x 16</option>
												<option value="6">Pingadeira 1.52 x 14</option>
												<option value="7">Pingadeira 1.52 x 16</option>
												<option value="8">Espelho</option>
												<option value="9">Rodapé</option>
												<option value="10">Soleira</option>
											</optgroup>
										</select>
									</div>
							</div>

							<div class="row">
								
								<div class="col-md-4">
											<label for="quantidade">Quantidade</label>
											<input type="number" id="quantidade" placeholder="0" class="form-control" 
											ref="quantidade"
											v-model="item.quantidade" step="1" min="1" max="99" />
								</div>

									<div class="col-md-4">
											<label for="altura">Largura (mts)</label>
											<input type="number" id="altura" placeholder="0" class="form-control" 
											v-model="item.altura" step=".01" ref="altura" />
									</div>

									<div class="col-md-4">
											<label for="comprimento">Comprimento (mts)</label>
											<input type="number" id="comprimento" placeholder="0" class="form-control" 
											v-model="item.comprimento" step=".01" ref="comprimento" />
									</div>		
							</div>	

							<div class="row">
								<div class="col-md-6 mb-3">
									<label for="firstName">Descrição da peça</label>
									<input type="text" class="form-control" id="firstName" 
										placeholder="Informação da peça" v-model="item.descricao">
										<small class="text-muted">Informação adicional da peça ou local onde será instalada</small>
									<div class="invalid-feedback">
										A informação da peça é obrigatória
									</div>
								</div>

								<div class="col-md-6">
									<label for="acabamento">Acabamento</label>
									<select class="form-control" id="acabamento" v-model="item.acabamento">
										<option value="0">Selecione o acabamento</option>
										<option v-for="acabamento in acabamentos" value="acabamento.id" v-bind:key="acabamento.id">{{ acabamento.descricao }}</option>
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
											@click="addItem(item)">
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
										<small class="text-muted">Será acrescido um valor de R$ 50,00 de frete!</small>
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
			<FlashMessage></FlashMessage>
	</main>
</template>

<script>
export default {
  name: 'Content',
  props: {},
	data: function(){
		return {
			item : this.inicializarItem(),
			items : [],
			dataAtual : new Date().toLocaleString(),
			total : 0,
			cliente : "",
			pedra : 0,
			acabamentos : [
				{ id: 1, descricao: "Reto", pecas : ["soleira"] },
				{ id: 2, descricao: "Meia esquadria", pecas : ["pingadeira"] },
				{ id: 3, descricao: "Duplo lixado", pecas : [] },
				{ id: 4, descricao: "Duplo arredondado", pecas : [] },
				{ id: 5, descricao: "Meia cana", pecas : ["soleira"] },
				{ id: 6, descricao: "Baulado", pecas : [] }
			]
		}
	},
	methods: {
		alterarTipoProduto(valor){
			if(valor == 4){
					this.item.altura = 1.02;
					this.item.comprimento = 0.16;
					this.item.acabamento = 1;
			} else if(valor == 5){		
					this.item.altura = 1.22;
					this.item.comprimento = 0.16;
					this.item.acabamento = 1;
			} else if(valor == 6){
					this.item.altura = 1.52;
					this.item.comprimento = 0.14;
					this.item.acabamento = 1;
			} else if(valor == 7) {
					this.item.altura = 1.52;
					this.item.comprimento = 0.16;
					this.item.acabamento = 1;
			}
			this.$refs.quantidade.focus()
		},

		addItem(item){

			if(this.pedra == null || this.pedra == 0){
				this.flashMessage.error({ title: 'Erro:', message: "É necessário escolher a cor da pedra!" });
				this.$refs.pedra.focus();
				return;
			}

			if(item.tipoPeca == null || item.tipoPeca == 0){
				this.flashMessage.error({ title: 'Erro:', message: "O produto/peça deve ser selecionado!" });
				this.$refs.produto.focus();
				return;
			}

			if(item.altura == null || item.altura <= 0){
				this.flashMessage.error({ title: 'Erro:', message: "A largura não pode ser menor que 0!" });
				this.$refs.altura.focus();
				return;
			}

			if(item.comprimento == null || item.comprimento <= 0){
				this.flashMessage.error({ title: 'Erro:', message: "O comprimento não pode ser menor que 0!" });
				this.$refs.comprimento.focus();
				return;
			}

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
			if(item.quantidade <= 0) item.quantidade = 1;

			item.total = item.preco * item.quantidade;
		
			this.items.push(item);
			this.item = this.inicializarItem();
			this.total = (parseFloat(this.total) + parseFloat(item.total)).toFixed(2);

			this.$refs.produto.focus()
		},

		inicializarItem(){
			return {
				quantidade : 1,
				acabamento : 0,
				pedra : 0
			}
		}
	},


}
</script>

<style>
</style>